## Multicast 통신

### Multicast 통신 매커니즘 -> 그룹 Join을 통한 데이터그램패킷 전송 및 수신

https://user-images.githubusercontent.com/82013205/219537649-c73302b6-4910-44dd-86eb-5d11c4b3425c.png

| 번호 |                             동작                             |
| ---- | :----------------------------------------------------------: |
| 1    | 각 클라이언트는 Multicast 를 지원하는 <span style="color:#ff5a54">MulticastSocket 생성</span><br />(<span style="color:#ff5a54">바인딩 정보 필수</span> 포함 (바인딩 port 미지정시 비어있는 port 자동 바인딩)) |
| 2    | 생성 MulticastSocket을 멀티캐스팅 IP(<span style="color:#ff5a54">클래스 D</span>)를 가지는 Multicast Group에 조인 (<span style="color:#ff5a54">joinGroup</span>(InetAddress)) |
| 3    | (송신) 보낼 데이터를 <span style="color:#ff5a54">DatagramPacket</span>에 담아 MulticastSocket을 통해 보내기 (<span style="color:#ff5a54">send(.)</span>) |
| 4    | (수신) Multicast Group에 패킷이 전달되면 패킷의 <span style="color:#ff5a54">수신 포트로 바인딩</span> 된 조인된 <span style="color:#ff5a54">모든 Client</span>에게 패킷 전송(<span style="color:#ff5a54">receive(.)</span>) |



### <span style="color:#ff5a54">송신</span> 매커니즘

https://user-images.githubusercontent.com/82013205/219538932-1c52d960-4465-4973-a428-fa2ea3c8892c.png

#### <span style="color:#ff5a54">send(.)</span> : Multicast Group에 등록(joinGroup())이 안되어 있어도 전송 가능

#### joinGroup(.) -> <span style="color:#ff5a54">send(.)</span> : Multicast Group에 등록되어 있는 경우 DatagramPacket의 수신지 port = MulticastSocket 바인딩 port 이면 자신이 보낸 데이터를 자신도 받음

https://user-images.githubusercontent.com/82013205/219539467-ff180b91-3946-4661-a150-28538e7f390d.png

### <span style="color:#ff5a54">MulticastSocket</span> -> 가상의 멀티캐스트 그룹에 가입(join)할 수 있는 소켓으로 멀티캐스트을 지원

#### MulticastSocket 객체 생성

| 생성자                                  | 동작                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| MulticastSocket()                       | 포트를 지정하지 않는 경우 비워져 있는 <span style="color:#ff5a54">포트로 자동 바인딩</span><br />(주로 <span style="color:#ff5a54">송신하는 경우에 사용</span>) |
| MulticastSocket(int port)               | 매개변수로 전달된 port로 가상의 Multicast Group에서 수신할 포트를 바인딩<br />(즉, Multicast Group에 도착하는 패킷 중 포트가 port인 데이터를 수신) |
| MulticastSocket(SocketAddress bindaddr) | 매개변수로 전달된 SocketAddress로 바인딩 (이때, 데이터수신을 위해서는 127.0.0.1 또는 localhost가 아닌 실제 IP를 정보를 포함하는 <span style="color:#ff5a54">InetAddress을 지정</span>) |

#### MulticastSocket의 주요메서드

| 반환타입       | 메서드 이름                                                  | 동작                                                         |
| -------------- | ------------------------------------------------------------ | :----------------------------------------------------------- |
| int<br />void  | getTimeToLive()<br />setTimeToLive(int ttl)                  | <span style="color:#ff5a54">패킷의 생존 기간 읽기 및 설정</span><br />0~255까지 가능 (이외 illegalArgumentException 발생) : 단말(스위치, 라우터 등)을 통과할 때 마다 숫자 감소 (1(<span style="color:#ff5a54">default</span>)인 경우 내부 네트워크에서만 사용) |
| void<br />void | joinGroup(InetAddress mcastaddr)<br />leaveGroup(InetAddress mcastaddr) | <span style="color:#ff5a54">Multicast Group에 가입 및 해제</span><br />그룹에 가입한 경우 이후의 바인딩 된 포트로 들어오는 모든 패킷 수신 가능<br />(해체(leaveGroup())시 수신 불가) |
| void<br />void | Send(DatagramPacket p)<br />receive(DatagramPacket p)        | <span style="color:#ff5a54">데이터그램 패킷의 전송과 수신:</span><br />상위 클래스인 DatagramSocket의 메서드로 데이터그램의 패킷 전송과 수신 수행 |



- 브로드캐스팅 : 멀티캐스팅 주소가 아니라 브로드캐스팅 주소(255.255.255.255)로 데이터그램 패킷을 전송하는 경우 내부 네트워크의 모든 단말에 전달 (join 등의 과정 없음)



### <span style="color:#ff5a54">MulticastSocket</span> -> 가상의 멀티캐스트 그룹에 가입(join)할 수 있는 소켓으로 멀티캐스트을 지원

```java
public static void main(String[] args) {
    MulticastSocket mcs1 = null, mcs2 = null, mcs3 = null;
    try {
        mcs1 = new MulticastSocket();   // 비어있는 포트로 자동 바인딩
        mcs2 = new MulticastSocket(10000);
        mcs3 = new MulticastSocket(new InetSocketAddress("192.168.0.5", 10000));    // 일반적으로 멀티캐스트에서는 포트만 지정
    }catch (IOException e) {
        e.printStackTrace();
    }

    mcs1.getLocalSocketAddress();   // 0.0.0.0/0.0.0.0:55477
    mcs2.getLocalSocketAddress();   // 0.0.0.0/0.0.0.0:10000
    mcs3.getLocalSocketAddress();   // 192.168.0.5:10000

    try {
        mcs1.getTimeToLive();   // 1
        mcs1.setTimeToLive(50);
        mcs1.getTimeToLive();   // 50
    } catch (IOException e) {
        e.printStackTrace();
    }

    try{
        mcs1.joinGroup(InetAddress.getByName("234.234.234.234"));
        mcs2.joinGroup(InetAddress.getByName("234.234.234.234"));
        mcs3.joinGroup(InetAddress.getByName("234.234.234.234"));

        byte[] sendData = "안녕하세요".getBytes();

        DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, InetAddress.getByName("234.234.234.234"), 10000);
        mcs1.send(sendPacket);

        byte[] receivedData;
        DatagramPacket receivedPacket;

        receivedData = new byte[65508];
        receivedPacket = new DatagramPacket(receivedData, receivedData.length);
        mcs2.receive(receivedPacket);

        System.out.print("mcs2가 수신한 데이터 : " + new String(receivedPacket.getData()).trim());
        System.out.println(" 송신지 : " + receivedPacket.getSocketAddress());
        // mcs2가 수신한 데이터 : 안녕하세요 송신지 : /192.168.0.5:55477
        
        receivedData = new byte[65508];
        receivedPacket = new DatagramPacket(receivedData, receivedData.length);
        mcs3.receive(receivedPacket);

        System.out.print("mcs3가 수신한 데이터 : " + new String(receivedPacket.getData()).trim());
        System.out.println(" 송신지 : " + receivedPacket.getSocketAddress());
        // mcs3가 수신한 데이터 : 안녕하세요 송신지 : /192.168.0.5:55477
    } catch (UnknownHostException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

