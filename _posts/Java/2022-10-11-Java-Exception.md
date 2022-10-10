## 예외처리

### <span style="color:#ff5a54">예외(Exception)와 에러(Error)</span>의 차이

- 예외 : 연산오류, 포맷오류 등 상황에 따라 개발자가 <span style="color:#ff5a54">해결 가능</span>한 오류
- 에러 : JVM 자체의 오류로 개발자가 <span style="color:#ff5a54">해결할 수 없는</span> 오류
  Ex).OutOfMemoryError, ThreadDeath



### 일반 예외 -> Exception으로 부터 바로 상속

- 예외처리를 하지 않으면 컴파일 자체가 불가능.
- 강제로 try/catch문 작성 강요.

### 실행예외 -> RuntimeException으로 부터 바로 상속

- 예외처리를 하지 않아도 컴파일은 가능
- 실행중 예외가 발생하면 프로그램 종료.



### 예외처리.

- 예외 처리 프로세스 : try/catch(finally)로 처리.
- JVM은 해당 예외 객체를 매개변수로 하는 catch(.) 메서드를 호출하는 개념.
- catch문을 다중으로 사용하여 다중 예외처리가 가능.



### 예외(Exception)의 전가(throws)

- <span style="color:#ff5a54">예외처리를 자신이 호출된 지점으로 전가</span> (이 경우 예외처리는 전가받은 상위위치에서 처리)
- 메서드이름(...) <span style="color:#ff5a54">throws</span> 예외클래스

```java
//예외전가..
void throwsMethod() throws InterruptedException {
    Thread.sleep(1000);
}

// try/catch..
void exceptionMethod2() {
    try {
        Thread.sleep(1000);
    } catch (InterruptedException ie) {
        ie.printStackTrace();
    }
}
```





### 예외 클래스 사용자 정의.

#### 사용자 정의 예외 클래스 작성 및 발생방법

- 사용자 정의 예외 클래스 <span style="color:#ff5a54">작성방법 (Exception/RuntimeException 상속(생성자 2개 지정))</span>

- Exception 상속 : 일반예외(Checked Exception)으로 생성

  ```Java
  class MyException extends Exception {
      MyException() {}
      MyException(String s) {
          super(s);   // 부모생성자 호출.
      }
  }
  ```

- <span style="color:#ff5a54">RuntimeException</span> 상속 : 실행예외(UnChecked Exception)으로 생성.

```java
class MyRTException extends RuntimeException {
    MyRTException() {}
    MyRTException(String s) {
        super(s);   // 부모생성자 호출.
    }
}
```

두 예제다 s는 exception 메세지 이고 try/catch문에서 exception의 e.message()와 같은 동작을 한다.

- 사용자 정의 예외 클래스 <span style="color:#ff5a54">객체 생성</span>

```java
MyException me1 = new MyException();
MyException me2 = new MyException("예외메세지");
```

- 예외 발생 시키기

```java
throw me1;
throw me2;

throw new MyException();
throw new MyException("예외메세지");
```