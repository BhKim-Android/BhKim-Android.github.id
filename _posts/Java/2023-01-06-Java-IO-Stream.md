## IO (InputStream / OutputStream)

### System.out : 자바 API에서 제공하는 콘솔출력을 위한 <span style="color:#ff5a54">OutputStream(PrintStream) 객체</span> 



### System.out 콘솔 출력의 특징

- Java API에서 콘솔 <span style="color:#ff5a54">출력</span>용으로 <span style="color:#ff5a54">하나의 객체</span>를 생성하여 제공
  -> close()로 자원해제하면 이후 콘솔출력 불가
- <span style="color:#ff5a54">출력</span>시 <span style="color:#ff5a54">엔터(개행)</span>의 표현은 <span style="color:#ff5a54">\r + \n, \n</span> 모두 가능
- <span style="color:#ff5a54">write()</span> 메서드는 <span style="color:#ff5a54">버퍼에 쓰기</span>를 수행 + <span style="color:#ff5a54">flush()</span> 메서드는 <span style="color:#ff5a54">버퍼의 내용을 콘솔로 출력</span> (반드시 <span style="color:#ff5a54">flush()</span> 사용하여야 함)

```java
// write() + flush()
OutputStream os = System.out;
os.write('A');  // 출력내용 -> 버퍼
os.flush(); // 버퍼 -> 콘솔

OutputStream os = System.out;
os.write(65);  // 출력내용 -> 버퍼
os.flush(); // 버퍼 -> 콘솔
```

출력내용 --<span style="color:#ff5a54">write()</span>--> <span style="color:#ff5aff">출력 버퍼</span> --<span style="color:#ff5a54">flush()</span>--> <span style="color:#ff5aff">콘솔 출력</span>

출력값 동일 (A)



### <span style="color:#ff5a54">void write(int b)</span>, void flush(), void close()

```java
// OutputStream 생성(콘솔)
OutputStream os1 = System.out;

// byte 단위쓰기
os1.write('J');
os1.write('A');
os1.write('V');
os1.write('A');
os1.write(13);  // carriage retur : \r
os1.write(10);  // line feed : \n

os1.flush();
```

<span style="color:#ff5a54">다양한 flush()의 생략조건</span>

- 내부적으로 버퍼를 사용하지 않는 경우
- Write(int )를 사용하는 경우 write('\n')이 포함되는 경우 자동 flush();
- Write(byte[], ...)의 경우 자동 flush()

#### <span style="color:#ff5a54">굳이 구분하지 말고 write() 다음에는 flush()를 쓰는 버릇을 들이자</span>



### <span style="color:#ff5a54">void write(byte[] b), void write(byte[], int offset, int length)</span>, void flush(), void close()

```java
// OutputStream 생성(콘솔) - 영어
OutputStream os = System.out;

// n-byte 단위 쓰기 (byte[]의 처음 위치에서 부터 끝까지를 출력)
byte[] byteArray1 = "Hello!".getBytes();
os.write(byteArray1);
os.write('\n');
os.flush();	// Hello!

// n-byte 단위 쓰기 (byte[]의 offset 위치에서부터 length개수를 읽어와 출력)
byte[] byteArray2 = "Better the last smile than the first laughter".getBytes();
os.write(byteArray2, 7, 8);
os.flush();	// the last
```

```java
// OutputStream 생성(콘솔) - 한글
OutputStream os = System.out;

// n-byte 단위 쓰기 (byte[]의 처음 위치에서 부터 끝까지를 출력)
byte[] byteArray1 = "안녕하세요".getBytes(Charset.forName("MS949"));
os.write(byteArray1);
os.write('\n');
os.flush(); // 안녕하세요

// n-byte 단위 쓰기 (byte[]의 offset 위치에서부터 length개수를 읽어와 출력)
byte[] byteArray2 = "반갑습니다".getBytes(Charset.defaultCharset());
os.write(byteArray2, 4, 4);
os.flush(); // 습니
```
