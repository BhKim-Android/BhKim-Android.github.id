## TCP(Transmission Control Potocol) 통신

### Client와 Server간의 Text 교환

```java
public static void main(String[] args) throws IOException {
		Socket socket = null;
		try {
    		socket = new Socket(InetAddress.getByName("localhost"), 10000); // 원격지 연결정보(IP, Port) 포함 소켓 생성
    
    		// Server에 접속완료
    		// 접속 server 주소 : localhost/127.0.0.1:10000

    		// 원격 스트림 생성
    		DataInputStream dis = new DataInputStream(new BufferedInputStream(socket.getInputStream()));
        DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(socket.getOutputStream()));
    
    		// 데이터 읽기 및 쓰기
    		dos.writeUTF("안녕하세요");
    		dos.flush();
    		String str = dis.readUTF();
		} catch (UnknownHostException e) {}
		catch (IOException e) {}
}
```



### Client와 Server간의 File 전송

```java
public static void main(String[] args) throws IOException {
    Socket socket = null;
    try {
        socket = new Socket(InetAddress.getByName("localhost"), 10000); // 원격지 연결정보(IP, Port) 포함 소켓 생성

        // Server에 접속완료
        // 접속 server 주소 : localhost/127.0.0.1:10000

        // 원격 스트림 생성
        DataInputStream dis = new DataInputStream(new BufferedInputStream(socket.getInputStream()));
        DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(socket.getOutputStream()));

        // 파일 읽기 및 쓰기
        File file = new File(filePath);
        FileInputStream fis = new FileInputStream(file);
        BufferedInputStream bis = new BufferedInputStream(fis);

        dos.writeUTF(file.getName());   // 파일이름 전송
        
        // 파일전송
        byte[] data = new byte[2048];
        int len;
        while ((len = bis.read(data)) != -1) {
            dos.writeInt(len);  // 전송데이터의 길이
            dos.write(data, 0, len);    // 전송데이터
            dos.flush();
        }
        
        dos.writeInt(-1);   // 데이터 전송완료 알림
        dos.flush();
        
        String str = dis.readUTF(); // 파일전송 완료
    } catch (UnknownHostException e) {}
    catch (IOException e) {}
}
```

```java
public static void main(String[] args) throws IOException {
    ServerSocket serverSocket = null;

    serverSocket = new ServerSocket(10000);

    // Client 접속 대기

    Socket socket = serverSocket.accept();

    // Client 연결 수락
    // 접속 client 주소 : 127.0.0.1:54300

    // 원격 스트림 생성
    DataInputStream dis = new DataInputStream(new BufferedInputStream(socket.getInputStream()));
    DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(socket.getOutputStream()));

    String receivedFileName = dis.readUTF();    // 전송파일이름 읽기

    // 파일쓰기 객체생성
    File file = new File(filePath + receivedFileName);
    FileOutputStream fos = new FileOutputStream(file);
    BufferedOutputStream bos = new BufferedOutputStream(fos);

    // 파일 수신
    byte[] data = new byte[2048];
    int len;
    while ((len = dis.readInt()) != -1) {
        dis.read(data, 0, len);
        bos.write(data, 0, len);
        bos.flush();
    }

    // 파일 수신 완료.
    dos.writeUTF("파일 전송 완료");
    dos.flush();
}
```