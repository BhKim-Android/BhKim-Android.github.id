## UDP(User Datagram Protocol) 통신

### DatagramPacket -> UDP 통신에서 보내고자 하는 <span style="color:#ff5a54">데이터를 포장(소포박스개념)</span>하는 클래스 (하나의 패킷당 최대 65,508 byte)

#### DatagramPacket 객체 생성 : byte[]의 데이터를 포함

| 생성자                                                       | 동작                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| DatagramPacket(byte[] buf, int length)<br />DatagramPacket(byte[] buf, int offset, int length) | <span style="color:#ff5a54">주소 없이</span> 단순히 데이터만 저장하는 패킷<br />(일반적으로 수신측에서 수신할 패킷을 저장하는데 사용) |
| DatagramPacket(btye[] buf, int length, InetAddress address, int port)<br />DatagramPacket(byte[] buf, int offset, int length, InetAddress address, int port) | <br />데이터의 정보에 추가하여 수신측 주소 정보를 InetAddress와 port 번호로 추가한 패킷 (소포 포장지에 주소를 써 놓는 개념) |
| DatagramPacket(byte[] buf, int length, SocketAddress address)<br />DatagramPacket(byte[] buf, int offset, int length, SocketAddress address) | <br />데이터의 정보에 추가하여 수신측 주소 정보를 SocketAddress(InetAddress + Port)로 추가한 패킷 (소포포장지에 주소를 써 놓는 개념) |



#### DatagramPacket 주요 메서드

|          반환타입          | 메서드 이름                                                  | 동작                                                         |
| :------------------------: | :----------------------------------------------------------- | :----------------------------------------------------------- |
|   InetAddress<br />void    | getAddress()<br />setAddress(InetAddress iaddr)              | <span style="color:#ff5a54">원격지 IP주소 읽기 및 설정:</span><br />InetAddress 타입으로 설정된 수신지 주소 읽기 및 쓰기<br />(수신지 주소가 없는 경우 getAddress()는 null을 리턴) |
|       int<br />void        | getPort()<br />setPort(int iport)                            | <span style="color:#ff5a54">원격지 Port 읽기 및 설정:</span><br />Int 타입으로 설정된 수신지 포트 읽기 및 쓰기<br />(수신지 주소가 없는 경우 getPort()는 -1을 리턴) |
|  SocketAddress<br />void   | getSocketAddress()<br />setSocketAddress(SocketAddress address) | <span style="color:#ff5a54">원격지 SocketAddress 읽기 및 설정:</span><br />SocketAddress 타입으로 설정된 수신지 IP와 Port 읽기 및 쓰기<br />(수신지 SocketAddress가 없는 경우 IllegalArgumentException) |
| byte[]<br />void<br />void | getData()<br />setData(byte[] buf)<br />setData(byte[] buf, int offset, int length) | <span style="color:#ff5a54">패킷에 포함되는 데이터 읽기 및 설정:</span><br />byte[] 타입으로 패킷에 포함된 데이터를 읽기 및 쓰기, 이때 getData()의 리턴값은 offset, length와 관계 없이 buf값 리턴<br />(패킷당 전송가능한 최대 raw 데이터=65,508 bytes) |
|   int<br />int<br />void   | getLength()<br />getOffset()<br />setLength(int length)      | <span style="color:#ff5a54">데이터 길이 읽기 및 설정 및 오프셋 읽기:</span><br />패킷에 포함되는 데이터의 길이 읽기 및 쓰기와 오프셋 읽기 |



```java
// 송신데이터: 최대바이트수 65508byte (64Kbyte) : ipv4 (65536-20(IP header) - 8(UDP header) = 65505)
byte[] buf = "UDP-데이터그램패킷".getBytes();

DatagramPacket dp1 = new DatagramPacket(buf, buf.length);   // UDP-데이터그램패킷
DatagramPacket dp2 = new DatagramPacket(buf, 4, buf.length - 4);    // 데이터그램패킷 (단, getData()는 전체 포함)

// 수신측 주소가 inetAddress와 port로 포함되어 있는 경우
DatagramPacket dp3 = null, dp4 = null;
try {
    dp3 = new DatagramPacket(buf, buf.length, InetAddress.getByName("localhost"), 10000);   // data = UDP-데이터그램패킷
    dp4 = new DatagramPacket(buf, 4, buf.length-4, InetAddress.getByName("localhost"), 10000);  // data = 데이터그램패킷
} catch (UnknownHostException e) {
    e.printStackTrace();
}

// 수신측 주소가 SocketAddress로 포함 되어 있는 경우
DatagramPacket dp5 = null, dp6 = null;
dp5 = new DatagramPacket(buf, buf.length, new InetSocketAddress("localhost", 10000));
dp6 = new DatagramPacket(buf, 4, buf.length - 4, new InetSocketAddress("localhost", 10000));

dp1.getAddress();   // null
dp1.getPort();  // -1
dp3.getAddress();   // localhost/127.0.0.1
dp3.getPort();  // 10000
dp3.getSocketAddress(); // localhost/127.0.0.1:10000

new String(dp1.getData());  // UDP-데이터그램패킷
new String(dp2.getData());  // UDP-데이터그램패킷 (len와 상관없이 항상 전체데이터 리턴)
new String(dp2.getData(), dp2.getOffset(), dp2.getLength());    // 데이터그램패킷

dp1.setData("안녕하세요".getBytes());
new String(dp1.getData());  // 안녕하세요
```