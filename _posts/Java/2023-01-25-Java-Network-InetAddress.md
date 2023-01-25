## 주소 저장 클래스

### InetAddress : IP주소(호스트이름) 저장 및 관리 클래스 (Port 번호 관리 못함)

- getByName(String host) : Host 이름과 해당 IP 주소 저장 객체 리턴
- getByAddress(byte[] addr) : 입력 IP주소 저장 객체 리턴 (128 이상의 경우 (byte) 캐스팅 필요)
- getByAddress(String host, byte[] addr) : Host 이름과 입력 IP주소 저장 객체 리턴 (IP 주소가 128 이상의 경우 (byte) 타입으로 캐스팅 필요)
- getLocalHost() : 로컬 호스트 IP저장 객체 리턴
- getLoopbackAddress() : 루프백(loopback) IP (127.0.0.1) 저장 객체 리턴

```java
InetAddress ia1 = InetAddress.getByName("www.google.com");  // www.google.com / 172.217.161.132
InetAddress ia2 = InetAddress.getByAddress(new byte[] {(byte)172, (byte) 217, (byte) 161, (byte) 132}); // 172.217.161.132
InetAddress ia3 = InetAddress.getByAddress("www.google.com", new byte[] {(byte)172, (byte) 217, (byte) 161, (byte) 132}); // www.google.com/172.217.161.132

InetAddress ia4 = InetAddress.getLocalHost();   // BHKIM/192.168.123.159
InetAddress ia5 = InetAddress.getLoopbackAddress(); // localhost/127.0.0.1

InetAddress[] ia6 = InetAddress.getAllByName("www.naver.com");  // [www.naver.com/210.89.164.90, www.naver.com/210.89.160.88]


```

```java
byte[] address =ia1.getAddress();
Arrays.toString(address);   // [-84, -39, -95, -124]
ia1.getHostAddress();   // 172.217.161.132
ia1.getHostName();  // www.google.com
ia1.isReachable(1000);  // true
ia1.isLoopbackAddress();    // false
ia1.isMulticastAddress();   // false
InetAddress.getByAddress(new byte[] {127,0,0,1}).isLoopbackAddress();   // true
InetAddress.getByAddress(new byte[] {(byte) 225, (byte) 225, (byte) 225, (byte) 225}).isMulticastAddress();    // true
```