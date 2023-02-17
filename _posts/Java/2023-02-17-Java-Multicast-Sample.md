## Multicast 통신 예제

### Client간 Text 교환

#### <span style="color:#ff5a54">Multicast 통신 예제1</span> -> client간 text 전송 (두 개의 클라이언트가 <span style="color:#ff5a54">Multicast Group에 조인</span>하고 데이터 송수신)

<img src="https://user-images.githubusercontent.com/82013205/219555449-199b087d-c086-472c-82fd-217adc1593e2.png">

Multicast Group에 조인된 경우 <span style="color:#ff5a54">같은 포트</span>에 바인딩 되어 있으면 자신이 보낸 패킷도 <span style="color:#ff5a54">다시 자신이 수신함</span> 두 클라이언트가 <span style="color:#ff5a54">다른 포트</span>로 바인딩되어 있으면 자신이 보낸 데이터를 자신은 <span style="color:#ff5a54">받지 않음</span>

```java
public static void main(String[] args) {
    System.out.println("<<ClientA>> - Text");
  
    // 멀티캐스팅 주소지 생성
    InetAddress multicastAddress = null;
    try {
        multicastAddress = InetAddress.getByName("234.234.234.234");
    } catch (UnknownHostException e) {
        e.printStackTrace();
    }
    int multicastPort = 10000;

    // 멀티캐스트소켓 생성
    MulticastSocket mcs = null;
    try {
        mcs = new MulticastSocket(multicastPort);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 멀티캐스트 그룹에 조인
    try {
        mcs.joinGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }
    
    // 전송 데이터그램 패킷 생성 + 전송
    byte[] sendData = "안녕하세요!(ClientA)".getBytes();
    
    DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }
    
    // 데이터그램 패킷 수신
    receiveMessage(mcs);    // 자기자신(ClientA)이 보낸 데이터 수신
    receiveMessage(mcs);    // 상대편(ClientB)이 보낸 데이터 수신
    
    // 멀티캐스트 그룹 나가기
    try {
        mcs.leaveGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }
    
    // 소켓 닫기
    mcs.close();
}
```

```java
public static void main(String[] args) {
    System.out.println("<<ClientB>> - Text");
    // 멀티캐스팅 주소지 생성
    InetAddress multicastAddress = null;
    try {
        multicastAddress = InetAddress.getByName("234.234.234.234");
    } catch (UnknownHostException e) {
        e.printStackTrace();
    }
    int multicastPort = 10000;

    // 멀티캐스트소켓 생성
    MulticastSocket mcs = null;
    try {
        mcs = new MulticastSocket(multicastPort);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 멀티캐스트 그룹에 조인
    try {
        mcs.joinGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 전송 데이터그램 패킷 생성 + 전송
    byte[] sendData = "안녕하세요!(ClientB)".getBytes();

    DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 데이터그램 패킷 수신
    receiveMessage(mcs);    // 자기자신(ClientA)이 보낸 데이터 수신
    receiveMessage(mcs);    // 상대편(ClientB)이 보낸 데이터 수신

    // 멀티캐스트 그룹 나가기
    try {
        mcs.leaveGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 소켓 닫기
    mcs.close();
}
```

```java
static void receiveMessage(MulticastSocket mcs) {
    byte[] receivedData;
    DatagramPacket receivedPacket;

    receivedData = new byte[65508];
    receivedPacket = new DatagramPacket(receivedData, receivedData.length);

    try {
        mcs.receive(receivedPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    System.out.println("보내온 주소 : " + receivedPacket.getSocketAddress());
    System.out.println("보내온 내용 : " + new String(receivedPacket.getData()).trim());
}
```

<<ClientA>> - Text
보내온 주소 : /192.168.0.5:10000
보내온 내용 : 안녕하세요!(ClientA)
보내온 주소 : /192.168.0.5:10000
보내온 내용 : 반갑습니다!(ClientB)

<<ClientB>> - Text
보내온 주소 : /192.168.0.5:10000
보내온 내용 : 안녕하세요!(ClientA)
보내온 주소 : /192.168.0.5:10000
보내온 내용 : 반갑습니다!(ClientB)



### Client간 File 교환

#### <span style="color:#ff5a54">Multicast 통신 예제2</span> -> client간 file 전송 (두 개의 클라이언트가 <span style="color:#ff5a54">Multicast Group에 조인</span>하고 File 송수신)

<img src="https://user-images.githubusercontent.com/82013205/219557590-6eb54775-6561-4e52-b490-237593c7c6d8.png">

Multicast Group에 조인된 경우 <span style="color:#ff5a54">같은 포트</span>에 바인딩 되어 있으면 자신이 보낸 패킷도 <span style="color:#ff5a54">다시 자신이 수신함</span> 두 클라이언트가 <span style="color:#ff5a54">다른 포트</span>로 바인딩되어 있으면 자신이 보낸 데이터를 자신은 <span style="color:#ff5a54">받지 않음</span>

```java
public static void main(String[] args) {
    System.out.println("<<ClientA>> - File");

    // 멀티캐스팅 주소지 생성
    InetAddress multicastAddress = null;
    try {
        multicastAddress = InetAddress.getByName("234.234.234.234");
    } catch (UnknownHostException e) {
        e.printStackTrace();
    }
    int multicastPort = 10000;

    // 멀티캐스트소켓 생성
    MulticastSocket mcs = null;
    try {
        mcs = new MulticastSocket(multicastPort);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 파일 로딩
    File file = new File(filePath);
    BufferedInputStream bis = null;

    try {
        bis = new BufferedInputStream(new FileInputStream(file));
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }

    // 파일데이터 전송
    DatagramPacket sendPacket = null;

    // 파일이름 전송
    String fileNmae = file.getName();
    sendPacket = new DatagramPacket(fileNmae.getBytes(), fileNmae.length(), multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }
    System.out.println(fileNmae + " 파일 전송 시작");

    // 파일 시작 태그를 전송 (/start)
    String startSign = "/start";
    sendPacket = new DatagramPacket(startSign.getBytes(), startSign.length(), multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 실제 파일 데이터 전송 (2048사이즈로 나누어서 파일 전송)
    int len;
    byte[] filedata = new byte[2048];
    try {
        while ((len = bis.read(filedata)) != -1) {
            sendPacket = new DatagramPacket(filedata, len, multicastAddress, multicastPort);
            mcs.send(sendPacket);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 파일 시작 태그를 전송(/end)
    String endSign = "/end";
    sendPacket = new DatagramPacket(endSign.getBytes(), endSign.length(), multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 멀티캐스트 그룹에 조인
    try {
        mcs.joinGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 데이터 수신 대기
    receiveMessage(mcs);

    // 멀티캐스트 그룹 나가기
    try {
        mcs.leaveGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 소켓 닫기
    mcs.close();
}
```

```java
public static void main(String[] args) {
    System.out.println("<<ClientB>> - File");

    // 멀티캐스팅 주소지 생성
    InetAddress multicastAddress = null;
    try {
        multicastAddress = InetAddress.getByName("234.234.234.234");
    } catch (UnknownHostException e) {
        e.printStackTrace();
    }
    int multicastPort = 10000;

    // 멀티캐스트소켓 생성
    MulticastSocket mcs = null;
    try {
        mcs = new MulticastSocket(multicastPort);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 파일 로딩
    File file = new File(filePath);
    BufferedInputStream bis = null;

    try {
        bis = new BufferedInputStream(new FileInputStream(file));
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }

    // 파일데이터 전송
    DatagramPacket sendPacket = null;

    // 파일이름 전송
    String fileNmae = file.getName();
    sendPacket = new DatagramPacket(fileNmae.getBytes(), fileNmae.length(), multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }
    System.out.println(fileNmae + " 파일 전송 시작");

    // 파일 시작 태그를 전송 (/start)
    String startSign = "/start";
    sendPacket = new DatagramPacket(startSign.getBytes(), startSign.length(), multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 실제 파일 데이터 전송 (2048사이즈로 나누어서 파일 전송)
    int len;
    byte[] filedata = new byte[2048];
    try {
        while ((len = bis.read(filedata)) != -1) {
            sendPacket = new DatagramPacket(filedata, len, multicastAddress, multicastPort);
            mcs.send(sendPacket);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 파일 시작 태그를 전송(/end)
    String endSign = "/end";
    sendPacket = new DatagramPacket(endSign.getBytes(), endSign.length(), multicastAddress, multicastPort);
    try {
        mcs.send(sendPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 멀티캐스트 그룹에 조인
    try {
        mcs.joinGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 데이터 수신 대기
    receiveMessage(mcs);

    // 멀티캐스트 그룹 나가기
    try {
        mcs.leaveGroup(multicastAddress);
    } catch (IOException e) {
        e.printStackTrace();
    }

    // 소켓 닫기
    mcs.close();
}
```

```java
static void receiveMessage(MulticastSocket mcs) {
    byte[] receivedData;
    DatagramPacket receivedPacket;

    receivedData = new byte[65508];
    receivedPacket = new DatagramPacket(receivedData, receivedData.length);

    try {
        mcs.receive(receivedPacket);
    } catch (IOException e) {
        e.printStackTrace();
    }

    System.out.println("보내온 주소 : " + receivedPacket.getSocketAddress());
    System.out.println("보내온 내용 : " + new String(receivedPacket.getData()).trim());
}
```

<<ClientA>> - File
ImageFileUsingMulticast.jpg 파일 전송 시작
보내온 주소 : /192.168.0.5:10000
보내온 내용 : (ClientB) 파일 수신 완료

<<ClientB>> - File
ImageFileUsingMulticast.jpg 파일 전송 시작
파일 수신 완료
