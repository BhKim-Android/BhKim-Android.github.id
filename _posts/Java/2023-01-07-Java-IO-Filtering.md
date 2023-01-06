## IO Filtering (FilterInputStream / FilterOutputStream)

### BufferedInputStream / BufferedOutputStream

입출력과정에서 메모리 버퍼를 사용함으로써 <span style="color:#ff5a54">속도 향상</span> 

```java
// 파일 생성
File orgfile = new File(filePath);
File copyfile1 = new File(filePath1);
File copyfile2 = new File(filePath2);

// Buffered(Input/Output)Stream을 사용하지 않은 경우
long start, end, time1, time2;
start = System.nanoTime();
try (
        InputStream is = new FileInputStream(orgfile);
        OutputStream os = new FileOutputStream(copyfile1);
) {
    int data;
    while ((data = is.read()) != -1) {
        os.write(data);
    }
} catch (IOException e) {
    e.printStackTrace();
}
end = System.nanoTime();
time1 = end - start;    // 2324488500

start = System.nanoTime();
try (
        InputStream is = new FileInputStream(orgfile);
        BufferedInputStream bis = new BufferedInputStream(is);
        OutputStream os = new FileOutputStream(copyfile2);
        BufferedOutputStream bos = new BufferedOutputStream(os);
) {
    int data;
    while ((data = bis.read()) != -1) {
        bos.write(data);
    }
    bos.flush();
} catch (IOException e) {
    e.printStackTrace();
}
end = System.nanoTime();
time2 = end - start;    // 13148700

time1/time2 // 176
```



### DataInputStream / DataOutputStream

입출력과정에서 다양한 <span style="color:#ff5a54">데이터타입(int, long, float, double, UTF8, ...)으로 입력 및 출력 수행</span>
Data (Input/Output)Stream : 다양한 타입으로 입력 및 출력

```java
// 파일 생성
File dataFile = new File(filePath);
if (!dataFile.exists()) dataFile.createNewFile();

// 데이터 쓰기 (FilterStream = DataOutputStream)
try (
        OutputStream os = new FileOutputStream(dataFile);
        DataOutputStream dos = new DataOutputStream(os);
) {
    dos.writeInt(35);
    dos.writeDouble(5.8);
    dos.writeChar('A');
    dos.writeUTF("안녕하세요");  // 디폴트 Charset과 관계없이 항상 UTF8로 생성
    dos.flush();    // #@-|333333 A {} 안녕하세요
} catch (IOException e) {}
```



### BufferedXXXStream + DataXXXStream <- <span style="color:#ff5a54">향상된 속도로 다양한 타입의 입출력 수행</span>

#### Buffered(Input/Output)Stream + Data(Input/Output)Stream

```java
// 파일 생성
File dataFile = new File(filePath);
if (!dataFile.exists()) dataFile.createNewFile();

// 데이터 쓰기 (FilterStream = DataOutputStream)
try (
        OutputStream os = new FileOutputStream(dataFile);
        BufferedOutputStream bos = new BufferedOutputStream(os);
        DataOutputStream dos = new DataOutputStream(bos);
) {
    dos.writeInt(35);
    dos.writeDouble(5.8);
    dos.writeChar('A');
    dos.writeUTF("안녕하세요");  // 디폴트 Charset과 관계없이 항상 UTF8로 생성
    dos.flush();    // #@-|333333 A {} 안녕하세요
} catch (IOException e) {}
```

```java
// 파일 생성
File dataFile = new File(filePath);
if (!dataFile.exists()) dataFile.createNewFile();

// 데이터 쓰기 (FilterStream = DataOutputStream)
try (
        InputStream is = new FileInputStream(dataFile);
        BufferedInputStream bis = new BufferedInputStream(is);
        DataInputStream dis = new DataInputStream(bis);
) {
    dis.readInt();  // 35
    dis.readDouble(); // 5.8
    dis.readChar(); // A
    dis.readUTF();  // 안녕하세요
} catch (IOException e) {}
```

### PrintStream <- 다양한 타입의 <span style="color:#ff5a54">출력에 특화된 클래스</span>자동 flush()기능 제공 

#### PrintStream : 다양한 타입으로 File/콘솔 출력

```java
// File 객체 생성 및 OutputStream 객체 생성(PrintStream)
File outFile1 = new File(filePath);
File outFile2 = new File(filePath);

// PrintStream(FileOutputStream(File))
try (
        OutputStream os1 = new FileOutputStream(outFile1);
        PrintStream ps = new PrintStream(os1);
) {
    ps.println(5.8);    // 5.8
    ps.print(3 + " 안녕 " + 12345 + "\n");    // 3 안녕 12345
    ps.printf("%d", 7).printf("%s %f", "안녕", 5.8);  // 7 안녕 5.800000
    ps.println();
} catch (IOException e) {
    e.printStackTrace();
}

try (
        PrintStream ps = new PrintStream(outFile2);
) {
    ps.println(5.8);    // 5.8
    ps.print(3 + " 안녕 " + 12345 + "\n");    // 3 안녕 12345
    ps.printf("%d ", 7).printf("%s %f", "안녕", 5.8); // 7 안녕 5.800000
    ps.println();
} catch (IOException e) {
    e.printStackTrace();
}

try (
        OutputStream os2 = System.out;
        PrintStream ps = new PrintStream(os2)
) {
    ps.println(5.8);    // 5.8
    ps.print(3 + " 안녕 " + 12345 + "\n");    // 3 안녕 12345
    ps.printf("%d ", 7).printf("%s %f", "안녕", 5.8); // 7 안녕 5.800000
    ps.println();
} catch (IOException e) {
    e.printStackTrace();
}
```
