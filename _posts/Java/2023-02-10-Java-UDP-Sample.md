```java
public static void main(String[] args) {
    DatagramSocket ds = null;

    try {
        ds = new DatagramSocket(10000);
        ds.connect(new InetSocketAddress("127.0.0.1", 20000));
    } catch (SocketException e) {
        e.printStackTrace();
    }

    // 파일로딩
    // 전송파일 입력스트림 연결
    File file = new File(filePath);
    BufferedInputStream fis = null;
    try {
        fis = new BufferedInputStream(new FileInputStream(file));
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }

    DatagramPacket sendPacket = null;

    // 파일이름 전송
    // 파일이름 저장 DatagramPacket생성 (원격지 주소 미포함)
    String fileName = file.getName();
    sendPacket = new DatagramPacket(fileName.getBytes(), fileName.length());
    try {
        ds.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 파일전송 시작 사인 전송(/start)
    // 파일데이터 시작 사인저장 DatatramPacket 생성(원격지 주소 미포함)
    String startSign = "/start";
    sendPacket = new DatagramPacket(startSign.getBytes(), startSign.length());
    try {
        ds.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 2048 사이즈로 나누어 실제 파일 데이터 전송.
    // 2048크기로 파일 읽어와 데이터패킷 생성 및 전송
    int len;
    byte[] filedata = new byte[2048];   // 최대는 65508byte이지만 안정적 네트워크 통신을 위해서 2048byte씩 잘라서 전송
    try {
        while ((len = fis.read(filedata)) != -1) {
            sendPacket = new DatagramPacket(filedata, len);
            ds.send(sendPacket);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 파일전송 종료 사인 전송
    // 파일데이터 종료 사인저장 DatagramPacket 생성 (원격지 주소 미포함)
    String endSign = "/end";
    sendPacket = new DatagramPacket(endSign.getBytes(), endSign.length());

    try {
        ds.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 데이터그램 패킷 수신
    // 수신을 위한 빈 DatagramPacket 생성
    byte[] receivedData = new byte[65508];
    DatagramPacket receivedPacket = new DatagramPacket(receivedData, receivedData.length);
    try {
        ds.send(receivedPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

```java
public static void main(String[] args) {
    DatagramSocket ds = null;
    try {
        ds = new DatagramSocket(20000);
        ds.connect(new InetSocketAddress("127.0.0.1", 10000));
    } catch (SocketException e) {
        e.printStackTrace();
    }

    // 데이터그램패킷 수신
    byte[] receivedData = null;
    DatagramPacket receivedPacket = null;

    // 파일이름 수신
    // 수신을 위한 빈 DatagramPacket 생성
    receivedData = new byte[65508];
    receivedPacket = new DatagramPacket(receivedData, receivedData.length);

    try {
        ds.receive(receivedPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 파일이름 수신 후 파일 출력스트림 연결
    String fileName = new String(receivedPacket.getData(), 0, receivedPacket.getLength());
    File file = new File(filePath + fileName);
    BufferedOutputStream bos = null;

    try {
        bos = new BufferedOutputStream(new FileOutputStream(file));
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }

    String startSign = "/start";
    String endSign = "/end";

    // 수신을 위한 빈 DatagramPacket 생성
    receivedData = new byte[65508];
    receivedPacket = new DatagramPacket(receivedData, receivedData.length);

    // "/start"부터 "/end"까지 반복적으로 데이터 수신 및 파일출력스트림에 기록
    try {
        ds.receive(receivedPacket);
        if (new String(receivedPacket.getData(), 0, receivedPacket.getLength()).equals(startSign)) {
            while (true) {
                ds.receive(receivedPacket);
                if (new String(receivedPacket.getData(), 0, receivedPacket.getLength()).equals(endSign)) break;
                bos.write(receivedPacket.getData(), 0, receivedPacket.getLength());
                bos.flush();
            }
        }
        bos.close();
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 전송데이터 생성 + Datagrampacket생성 (수신지 주소 포함)
    // 파일 수신완료 메시지 전송
    byte[] sendData = "파일 수신 완료".getBytes();
    DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length);

    try {
        ds.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```