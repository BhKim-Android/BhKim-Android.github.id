## UDP(User Datagram Protocol) 통신

### DatagramSocket -> UDP 통신에서 DatagramPacket을 네트워크를 통해 전송하거나 수신하는 <span style="color:#ff5a54">소켓(우편함 개념) 클래스</span>

DatagramSocket 객체 생성 : 소켓에 도착한 DatagramPacketdmf <span style="color:#ff5a54">바인딩 된 포트로 전달</span>하는 기능

| 생성자                                      | 동작                                                         |
| ------------------------------------------- | ------------------------------------------------------------ |
| DatagramSocket()                            | 기본생성자로 객체를 생성하는 경우 객체 생성 호스트에 <span style="color:#ff5a54">가용한 Port에 바인딩</span> 된 DatagramSocket() 객체 생성 |
| DatagramSocket(int port)                    | 입력 매개변수로 전달된 포트로 바인딩된 DatagramSocket() 객체 생성<br />\|(DatagramSocket에 DatagramPcket이 도착하면 바인딩된 Port로 전달) |
| DatagramSocket(int port, InetAddress laddr) | 매개변수로 전달된 InetAddress의 port에 바인딩 된 DatagramSocket 객체 생성 |
| DatagramSocket(SocketAddress bindaddr)      | 매개변수로 전달된 SocketAddress에 바인딩 된 DatagramSocket 객체 생성 |



### DatagramSocket 주요 메서드

| 반환타입                                | 메서드 이름                                                  | 동작                                                         |
| --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| void<br />void                          | Send(DatagramPacket p)<br />receive(DatagramPacket P)        | <span style="color:#ff5a54">DatagramPacket의 전송과 수신:</span> (IOException)<br />데이터를 포함한 DatagramPacket을 전송하고<br />수신된 DatagramPacket을 비어 있는 DatagramPacket으로 담아서 수신 |
| void<br />void<br />void                | Connect(InetAddress address, int port)<br />connect(SocketAddress addr)<br />disconnect() | <span style="color:#ff5a54">원격지 DatagramSocket 주소로 연결 및 연결해제 :</span><br />매개변수로 입력된 주소로 실제 연결 수행 및 연결 해제 |
| InetAddress<br />int                    | getInetAddress()<br />getPort()                              | <span style="color:#ff5a54">원격지 주소 정보:</span><br />원격지의 InetAddress와 Port값 읽기<br />(단, connect()를 통해 연결이 된 경우 유요한 값 리턴) |
| InetAddress<br />int<br />SocketAddress | getLocalAddress()<br />getLocalPort()<br />getLocalSocketAddress() | <span style="color:#ff5a54">로컬 주소 정보:</span><br />바인딩 된 로컬 호스트의 inetAddress, Port 또는 이 둘의 정보를 포함한<br />SocketAddress의 정보를 리턴 |
| int<br />int<br />void<br />void        | getReceiveBufferSize()<br />getSendBufferSize()<br />setReceiveBufferSize(int size)<br />setSendBufferSize(int size) | <span style="color:#ff5a54">송수신 버퍼크기의 읽기 및 설정</span><br />송수신시 사용되는 버퍼 크기의 읽기 및 설정 (SocketException)<br />(미설정시 송수신 버퍼크기는 65,536byte) |
| int<br />void                           | getSoTimeout()<br />setSoTimeout(int timeout)                | <span style="color:#ff5a54">수신 리스닝 시간:</span> DatagramPacket의 수신을 기다리는 시간 읽기 및 설정(timeout=0 이면 무한 대기)<br />(시간 완료시 SocketTimeoutException: 발생) |



```java
DatagramSocket ds1 = null, ds2 = null, ds3 = null, ds4 = null;

try {
    // port 미지정 / port만 지정
    ds1 = new DatagramSocket(); // 현재 호스트의 비워져 있는 포트로 자동 바인딩.
    ds2 = new DatagramSocket(10000);    // 10000 포트로 바인딩
    
    // 바인딩 정보(IP, Port) 포함
    ds3 = new DatagramSocket(10001, InetAddress.getByName("localhost"));
    ds4 = new DatagramSocket(new InetSocketAddress("localhost", 10002));
} catch (SocketException e) {
    e.printStackTrace();
}
```

|      | getLocalAddress() | getLocalPort() |
| ---- | ----------------- | -------------- |
| ds1  | 0.0.0.0/0.0.0.0   | 58895          |
| ds2  | 0.0.0.0/0.0.0.0   | 10000          |
| ds3  | /127.0.0.1        | 10001          |
| ds4  | /127.0.0.1        | 10002          |

```java
ds4.getInetAddress();   // null
ds4.getPort();  // -1

try {
    ds4.connect(new InetSocketAddress("localhost", 10003));
} catch (SocketException e) {
    e.printStackTrace();
}

ds4.getInetAddress();   // localhost/127.0.0.1
ds4.getPort();  //10003
```

```java
byte[] data1 = "수신지 주소가 없는 데이터그램 패킷".getBytes();
byte[] data2 = "수신지 주소가 있는 데이터그램 패킷".getBytes();
DatagramPacket dp1 = new DatagramPacket(data1, data1.length);
DatagramPacket dp2 = new DatagramPacket(data2, data2.length, new InetSocketAddress("localhost", 10002));

try {
    ds1.connect(new InetSocketAddress("localhost", 10002));
    ds2.connect(new InetSocketAddress("localhost", 10002));
    ds3.connect(new InetSocketAddress("localhost", 10002));
    ds1.send(dp1);
    ds2.send(dp1);
    ds3.send(dp1);
    ds1.disconnect();
    ds2.disconnect();
    ds3.disconnect();

    ds1.send(dp2);
    ds2.send(dp2);
    ds3.send(dp2);

    ds3.connect(new InetSocketAddress("localhost", 10002));
    ds3.send(dp2);
    ds3.disconnect();
} catch (SocketException e) {
    e.printStackTrace();
} catch (IOException e) {
    e.printStackTrace();
}

byte[] receivedData = new byte[65508];
DatagramPacket dp = new DatagramPacket(receivedData, receivedData.length);
try {
    for (int i = 0; i < 7; i++) {
        ds4.receive(dp);
        System.out.println("송신자 정보 " + dp.getAddress() + ":" + dp.getPort() + " -> " + new String(dp.getData()).trim());
    }
} catch (IOException e) {
    e.printStackTrace();
}

try {
    System.out.println("송신버퍼크기" + ds1.getSendBufferSize() + "수신버퍼크기" + ds1.getReceiveBufferSize());
} catch (SocketException e) {
    e.printStackTrace();
}
```

송신자 정보 /127.0.0.1:65170 -> 수신지 주소가 <span style="color:#ff5a54">없는</span> 데이터그램 패킷
송신자 정보 /127.0.0.1:10000 -> 수신지 주소가 <span style="color:#ff5a54">없는</span> 데이터그램 패킷
송신자 정보 /127.0.0.1:10001 -> 수신지 주소가 <span style="color:#ff5a54">없는</span> 데이터그램 패킷
송신자 정보 /127.0.0.1:65170 -> 수신지 주소가 <span style="color:#ff5a54">있는</span> 데이터그램 패킷
송신자 정보 /127.0.0.1:10000 -> 수신지 주소가 <span style="color:#ff5a54">있는</span> 데이터그램 패킷
송신자 정보 /127.0.0.1:10001 -> 수신지 주소가 <span style="color:#ff5a54">있는</span> 데이터그램 패킷
송신자 정보 /127.0.0.1:10001 -> 수신지 주소가 <span style="color:#ff5a54">있는</span> 데이터그램 패킷

송신버퍼크기 65536, 수신버퍼크기65536





```java
DatagramSocket ds = null;
try {
    ds = new DatagramSocket(10000); // 10000(port)에 바인딩 된 DatagramSocket 객체 생성
} catch (SocketException e) {
    e.printStackTrace();
}

// 원격지 정보 포함 DatagramPacket 생성
byte[] sendData = "안녕하세요".getBytes();
DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, new InetSocketAddress("127.0.0.1", 20000));

try {
    sendPacket.getData();   // 송신데이터 : 안녕하세요.
    ds.send(sendPacket);
} catch (IOException e) {
    e.printStackTrace();
}

// 데이터그램 패킷 수신.
// 수신을 위한 빈 DatagramPacket 생성
byte[] receivedData = new byte[65508];
DatagramPacket receivedPacket = new DatagramPacket(receivedData, receivedData.length);

try {
    ds.receive(receivedPacket);
} catch (IOException e) {
    e.printStackTrace();
}
```

