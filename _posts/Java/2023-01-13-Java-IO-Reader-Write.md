## char 단위의 입출력 (Reader/Write)

#### Reader : <span style="color:#ff5a54">char</span> 단위 <span style="color:#ff5a54">입력</span>을 수행하는 <span style="color:#AC58FA">추상클래스</span>

- int skip(long n) : n개의 char 스킵하기 (실제 스킵된 char 수 리턴)
- int read() : int(4byte)의 하위 <span style="color:#AC58FA">2byte</span>에 읽은 데이터를 저장하여 리턴
- inte read(char[] cbuf) : 읽은 데이터를 char[], cbuf에 저장하며 읽은 char수를 리턴
- Abstract int read(char[] cbuf, int off, int len) : length 개수만큼 읽은 데이터를 char[] cbuf의 offset 위치부터 저장 (<span style="color:#ff5a54">추상메서드</span>)
- abstract void close() : Reader의 자원 반환





#### Writer : <span style="color:#ff5a54">char</span> 단위 <span style="color:#ff5a54">출력</span>을 수행하는 <span style="color:#AC58FA">추상클래스</span>

- Abstract void flush() : 메모리 버퍼에 저장된 데이터 내보내기 (실제 출력 수행) (<span style="color:#ff5a54">추상메서드</span>)
- void write(int c) : int(4byte)의 하위 <span style="color:#AC58FA">2byte</span>를 메모리버퍼에 출력
- void write(char[] cbuf) : 매개변수로 념겨진 char[] cbuf 데이터를 메모리버퍼에 출력
- void write(String str) : 매개변수로 넘겨진 String 값을 메모리버퍼에 출력
- void write(String str, int off, int len) : str의 offset 위치에서부터 length개수를 읽어와 메모리버퍼에 출력
- abstract void write(char[] cbuf, int off, int len) : char[]의 offset 위치에서부터 length개수를 읽어와 출력 (<span style="color:#ff5a54">추상메서드</span>)
- abstract void close() : Reader의 자원 반환(<span style="color:#ff5a54">추상메서드</span>)



### FileReader/FileWriter로 Reader/Writer 객체 생성

#### FileReader/FileWriter : char 단위로 File 입출력 수행

- FileWriter로 파일쓰기 + FileReader로 데이터 읽기

```java
File readerwriterFile = new File(filePath);

try (Writer writer = new FileWriter(readerwriterFile);) {
    writer.write("안녕하세요\n".toCharArray());  // String -> char[]
    writer.write("Hello");
    writer.write("\r");
    writer.write("\n");
    writer.write("반갑습니다", 2, 3);
    writer.flush();
} catch (IOException e) {}

try (Reader reader = new FileReader(readerwriterFile);) {
    int data;
    while ((data == reader.read()) != -1) {
        System.out.print((char) data);                  // 안녕하세요
    }                                                   // Hello
} catch (IOException e) {}                              // 습니다.
```





### BufferedReader/BufferedWriter로 속도 개선

```java
File bufferedFile = new File(filePath);

try (Writer writer = new FileWriter(bufferedFile);) {
    writer.write("안녕하세요\n".toCharArray());  // String -> char[]
    writer.write("Hello");
    writer.write("\r");
    writer.write("\n");
    writer.write("반갑습니다", 2, 3);
    writer.flush();
} catch (IOException e) {}

try (Reader reader = new FileReader(bufferedFile);
     BufferedReader br = new BufferedReader(reader);) {
    String data;
    while ((data == br.readLine()) != null) {
        System.out.print(data);                         // 안녕하세요
    }                                                   // Hello
} catch (IOException e) {}                              // 습니다.
```



### InputStreamReader/OutputStreamWriter로 Reader/Writer객체 생성

#### InputStreamReader / OutputStreamWriter : byte 단위 입출력 -> char단위 입출력으로 변환

- Charset.defaultCharset() = <span style="color:#ff5a54">MS949</span>인 경우 -> (UTF - 8) 파일 읽어오기 -> MS949(ANSI)의 경우 결과 반대

```java
File inputStreamReader = new File(filePath);

try (Reader reader = new FileReader(inputStreamReader);) {
    int data;
    while ((data = reader.read()) != -1) {
        System.out.print((char) data);  // 한글 꺠짐..
    }
} catch (IOException e) {}

try (InputStream is = new FileInputStream(inputStreamReader);
     InputStreamReader isr = new InputStreamReader(is, "UTF-8")) {
    int data;
    while ((data = isr.read()) != -1) {System.out.print((char) data);}
    System.out.println("\n" + isr.getEncoding());      // UTF-8
} catch (IOException e) {}
```

- InputStreamReader : 콘솔 입력을 문자단위로 읽어오기 (<span style="color:#ff5a54">Charset.defaultCharset() = MS949</span> 기준)

```java
try {
    InputStreamReader isr = new InputStreamReader(System.in, "MS949");
    int data;
    while ((data = isr.read()) != '\r') {System.out.print((char) data);}
    System.out.println("\n" + isr.getEncoding());   // MS949
} catch (IOException e) {}

try {
    InputStreamReader isr = new InputStreamReader(System.in, "UTF-8");
    int data;
    while ((data = isr.read()) != '\r') { System.out.print((char) data);}   // 한글 깨짐
    System.out.println("\n" + isr.getEncoding());   // UTF-8
} catch (IOException e) {}
```

<span style="color:#ff5a54">System.in은 자원 반납 하지 않음</span>
Charset.defaultCharset() = UTF-8의 경우 결과 반대.

- OutputStreamWriter : <span style="color:#ff5a54">Charset.defaultCharset() = MS949</span>인 경우 디폴트 문자셋(<span style="color:#ff5a54">MS949(ANSI)</span>)과 <span style="color:#ff5a54">UTF-8</span> 문자셋으로 각각 <span style="color:#ff5a54">파일</span> 쓰기

```java
File outputStreamWriter1 = new File(filePath);

try(Writer writer = new FileWriter(outputStreamWriter1);) {
    writer.write("OutputStreamWriter1 예제파일입니다.\n".toCharArray());
    writer.write("한글과 영문이 모두 포함되어 있습니다.");
    writer.write("\n");
    writer.write("Good Bye!!!\n\n");
    writer.flush();
} catch (IOException e) {}
```

```java
File outputStreamWriter2 = new File(filePath);

try(OutputStream os  = new FileOutputStream(outputStreamWriter2, true);
    OutputStreamWriter osr = new OutputStreamWriter(os, "UTF-8");) {
    osr.write("OutputStreamWriter2 예제파일입니다.\n".toCharArray());
    osr.write("한글과 영문이 모두 포함되어 있습니다.");
    osr.write("\n");
    osr.write("Good Bye!!!\n\n");
    osr.flush();
    System.out.println(osr.getEncoding());  //UTF-8
} catch (IOException e) {}
```





### PrinterWriter로 Writer 객체 생성

### PrintWriter : PrinterStream과 같이 다양한 타입의 <span style="color:#ff5a54">출력에 특화된 클래스</span> 자동 flush()기능 제공 (autoflush = true) 또는 <span style="color:#ff5a54">자원 반납시 autoflush</span> 됨

- PrintWriter의 생성자 <span style="color:#ff5a54">(매개변수:파일, OutputStream, Writer)</span>

```java
// File -> PrintWriter
PrintWriter pw1 = new PrintWriter(new File(...));
// OutputStream1 -> PrintWriter
PrintWriter pw2 = new PrintWriter(new FileOutputStream(...));
// Console(System.out) -> PrintWriter
PrintWriter pw3 = new PrintWriter(System.out);
// Writer -> PrintWriter
PrintWriter pw4 = new PrintWriter(new FileWriter(...));
```















































