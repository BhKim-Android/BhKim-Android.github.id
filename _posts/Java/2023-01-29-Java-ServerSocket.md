## ServerSocket

TCP 통신에서 Socket으로 부터의 <span style="color:#ff5a54">연결요청을 수락</span>하는 서버열할의 클래스



ServerSocket() : 특정 포트 <span style="color:#ff5a54">바인딩(bind) 없이</span> 단순히 ServerSocket 객체만을 생성 (이후 binding 필요)

ServerSocket(int port) : 매개변수로 입력된 <span style="color:#ff5a54">port로 바인딩</span> 된 ServerSocket 객체 생성
(이후 ServerSocket으로의 연결요청이 오면 바인딩 된 port로 전달)

#### Binding vs Connet

- Binding은 수신 호스트에서 연결요청이 들어온 경우 해당 데이터를 전달할 연결 포트를 지정
- Connect는 원격지 특정 주소로의 연결을 수행



Bind(SocketAddress endpoint) : 바인딩 정보가 없는 서버 소켓에 바인딩 정보를 제공

isBound() : 바인딩 여부확인 ServerSocket이 바인딩 되어 있는지의 여부를 리턴

setSoTimeout(int timeout) : 연결요청 리스닝 시간 Socket으로 부터의 연결요청을 리스닝하는 시간의 설정 및 가져오기 (timeout=0 이면 무한 대기)

<span style="color:#ff5a54">accept()</span> : 연결요청 수락 연결요청이 수락된 후 통신을 위한 Socket 객체 리턴(연결수락까지 설정된 timeout 시간만큼 blocking)



```java
public static void main(String[] args) throws IOException {
    // 객체생성
    ServerSocket serverSocket1 = null;
    ServerSocket serverSocket2 = null;
    try {
        serverSocket1 = new ServerSocket();
        serverSocket2 = new ServerSocket(20000);
    } catch (IOException e) {}
    
    // ServerSocket 메서드
    // @bind
    serverSocket1.isBound();    // false
    serverSocket2.isBound();    // true
    try {
        serverSocket1.bind(new InetSocketAddress("127.0.0.1", 10000));
    } catch (IOException e) {}
    serverSocket1.isBound();    // true
    serverSocket2.isBound();    // true
}
```



```java
// @ 사용중인 TCP 포트 확인하기 (cmd: netstat -a_
for (int i = 0; i < 65536; i++) {
    try {
        ServerSocket serverSocket = new ServerSocket(i);
    } catch (IOException e) {
        System.out.println(i + "번째 포트 사용중 ...");
    }
}

// @accept() / setSoTimeout() / getSoTimeout()
// Client로 부터 TCP 접속 대기 (일반적으로 쓰레드 사용)
try {
    serverSocket1.setSoTimeout(2000);
} catch (SocketException e) {
    e.printStackTrace();
}
try {
    Socket socket = serverSocket1.accept();
} catch (IOException e) {
    try {
        System.out.println(serverSocket1.getSoTimeout() + "ms 시간이 지나 listening을 종료");
    } catch (IOException e) {
    }
}
```

n번째 포트 사용중 ...
(n+m)번째 포트 사용중 ...
(n+m)번째 포트 사용중 ...
(n+m)번째 포트 사용중 ...
(n+m)번째 포트 사용중 ...
...
2000시간이 지나 listening을 종료

