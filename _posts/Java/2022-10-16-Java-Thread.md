## 쓰레드

### 용어정리

- 프로그램 : <span style="color:#ff5a54">하드 디스크</span>에 저장된 파일
- 프로세스 : 파일을 실행하여 <span style="color:#ff5a54">메모리</span>에 로딩되어진 프로그램
- 스레드 : 프로세스 내부에서 <span style="color:#ff5a54">CPU</span>에 직접적인 데이터 전달

### Java Program상의 Thread

최초에 JVM main Thread를 생성하여 실행한다. (void main() {})

### multi-Thread

동시에 실행이 필요한 기능에 대해 서로 다른 Thread에서 각각의 기능을 처리.

- Concurrency(동시) : 서로 작업을 교차하여 순차적으로 진행하다보니 순차적인 처리와 작업 시간은 동일.
- Parallelism(병렬) : 각각의 작업을 서로다른 cpu 코어에서 각각 처리하여 처리 시간이 빠름.

#### Thread는 <span style="color:#ff5a54">동시성</span>과 <span style="color:#ff5a54">병렬성</span>을 가지고 수행.



### Thread의 생성 및 실행방법

#### Thread <span style="color:#ff5a54">생성 방법</span>

- Thread class를 <span style="color:#ff5a54">상속</span>받아 run() 메서드 재정의
- Runnable interface <span style="color:#ff5a54">구현</span> (추상메서드 <span style="color:#5a54ff">run()</span> 구현)
- Thread 생성자로 <span style="color:#ff5a54">Runnable 객체 전달</span>

#### Thread <span style="color:#ff5a54">실행</span>방법

- <span style="color:#ff5a54">Thread</span> class의 <span style="color:#ff5a54">start()</span> 메서드 호출

주의 1) 재정의한 메서드는 run()이지만 Thread의 실행은 <span style="color:#ff5a54">start()</span>메서드 호출
주의 2) Thread 객체는 재사용 할 수 없음 -> 하나의 객체는 <span style="color:#ff5a54">한번만 start() 가능</span>

1. Thread class를 <span style="color:#ff5a54">상속</span>받아 run() 메서드 재정의

```java
/**
 * Thread class를 상속받아 run()
 * 메서드 override한 클래스 정의 (또는 익명클래스)
 **/
class MyThread extends Thread {
    @Override
    public void run() {
        // Thread 작업내용
    }
}

class Main {
    public static void main(String[] args) {
        Thread myThread = new MyThread(); // Thread 객체 생성
        myThread.start(); // start() 메서드를 이용하여 Thread 실행.
    }
}
```

run() 호출시 단일쓰레드로 동작

2. <span style="color:#ff5a54">Runnable</span> interface 구현 -> Thread 생성자로 <span style="color:#ff5a54">Runnable 객체 전달</span>

```java
/**
 * Runnable interface를 구현 한 클래스 정의
 * (추상메서드 run()구현)
 **/
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // Thread 작업내용
    }
}

class Main {
    public static void main(String[] args) {
        Runnable runnable = new MyRunnable();   // Runnable 객체 생성
        Thread myThread = new Thread(runnable); // Thread 객체 생성 (생성자에 Runnable 객체전달)
        myThread.start();                       // start() 메서드를 이용하여 Thread 
    }
}
```

Runnable은 <span style="color:#ff5a54">함수적 인터페이스</span>로 내부에 <span style="color:#ff5a54">start() 메서드가 없어</span> start()를 위해서 Thread 객체가 필요함.



### Thread의 속성 (Thread 객체 가져오기 / 이름 / Thread 개수 / 우선순위 / 데몬설정)

- 현재 Thread 객체 참조 (Thread 클래스의 정적메서드)
  Static Thread Thread.currentThread() -> Thread 참조 객체가 없는 경우 사용.
- 실행중인 Thread 개수 가져오기 (Thread 클래스의 정적메서드)
  static int Thread.activeCount() -> 같은 ThreadGroup(생성된 Thread는 자신을 생성한 Thread와 같은 그룹에 속함)내의 active thread 수
- Thread 이름 설정 및 가져오기 (Thread 클래스의 인스턴스메서드)
  String setName(String name) -> Thread의 이름 설정하기
  String getName -> Thread의 이름 가져오기.
  (Thread의 이름을 지정하지 않은 경우 thread-0, thread-1, ..., thread-N과 같이 번호를 하나씩 증가시키면서 이름이 자동 부여)

```java
class Main {
    public static void main(String[] args) {
        // 객체 가져오기 + 쓰레드 수
        Thread curThread = Thread.currentThread();
        curThread.getName(); // 쓰레드에 부여된 이름.
        Thread.activeCount(); // 실행중인 쓰레드 수.
    }
}
```



- Thread 객체 우선순위 가져오기 (Thread 클래스의 인스턴스 메서드)
  int getPriority() -> Thread의 우선순위(Priority) 가져오기
- Thread 객체 우선순위 정하기 (Thread 클래스의 인스턴스 메서드)
  void setPriority(int priority) -> Thread의 우선순위 설정하기.
  <span style="color:#ff5a54">Thread.MAX_PRIORITY</span> : 가장 높은 우선순위
  <span style="color:#ff5a54">Thread.NORM_PRIORITY</span> : 기본 우선순위
  <span style="color:#ff5a54">Thread.MIN_PRIORITY</span> : 가장 낮은 우선순위



Thread <span style="color:#ff5a54">데몬</span> 설정 (Thread 클래스의 인스턴스 메서드) -> <span style="color:#ff5a54">주의) setDeamon()은 반드시 start() 전에 호출</span>
void setDeamon(boolean on) -> on이 true인 경우 Deamon Thread, false 일반 Thread

<span style="color:#ff5a54">일반 쓰레드</span> : 주쓰레드 종료여부와 상관없이 자신의 쓰레드가 종료되어야 프로세스 종료

<span style="color:#ff5a54">데몬 쓰레드</span> : 일반쓰레드가 모두 종료되면 작업이 완료되지 않았어도 함께 종료

```java
class Main {
    public static void main(String[] args) {
        Thread myThread1 = new MyThread();   // MyThread의 객체 생성.
        myThread1.setDaemon(false);  // myThread 데몬 쓰레드 설정여부 false
        myThread1.start();  // Main Thread 종료 후에도 Thread1의 작업을 전부 진행.

        Thread myThread2 = new MyThread();
        myThread2.setDaemon(true);  // 데몬 쓰레드 설정.
        myThread2.start();  // Main Thread와 myThread1이 모두 종료되는 시점에 같이 종료.

        try {
            Thread.sleep(3000); // 3초후 Main Thread 종료..
        } catch (InterruptedException e) {
            e.printStackTrace();    // Main Thread 종료..
        }
    }
}


class MyThread extends Thread {
    @Override
    public void run() {
        super.run();
        for (int i = 0; i < 6; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 데몬 쓰레드는 본인 쓰레드외에 다른 쓰레드가 살아있으면 계속 동작한다.



### Thread 동기화

<span style="color:#ff5a54">동기화(synchronized)</span>의 의미 (한번에 하나씩)

- 하나의 작업이 완전히 완료된 후 다른작업 수행
-  Cf. 비동기식: 하나의 작업 명령 이후(완료와 상관없이) 바로 다른 작업 명령을 수행

<span style="color:#ff5a54">동기화의 필요성</span> - 두개의 Thread가 하나의 <span style="color:#ff5a54">객체를 공유</span>하는 경우

```java
class MyData {
    int data = 3;

    public void plusData() {
        int myData = data;
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        data++;
    }
}

class PlusThread extends Thread {
    MyData myData;

    // 생성자..
    public PlusThread(MyData myData) {
        this.myData = myData;
    }

    @Override
    public void run() {
        super.run();
        myData.plusData();
    }
}

class Main {
    public static void main(String[] args) {
        MyData myData = new MyData();   // 공유객체

        Thread plusThread1 = new PlusThread(myData);    // 각각의 객체가 공통된 Thread객체를 공유
        plusThread1.start();    // 4

        Thread plusThread2 = new PlusThread(myData);
        plusThread2.start();    // 4
    }
}
```

plusThread1과 plusThread2는 각각 Thread로 동작.

- 공유객체를 <span style="color:#ff5a54">한번에 하나의 Thread만 사용</span>할 수 있도록 함 (하나의 Thread가 사용 중일때는 객체를 <span style="color:#ff5a54">lock</span>)

plusThread1가 모두 완료된후 plusThread2가 완료되도록 진행 -> <span style="color:#ff5a54">동기화</span>



#### 동기화 방법

- 메서드 동기화 : <span style="color:#ff5a54">동시에 두 개의 Thread가 동기화 메서드 사용불가</span>
- 블록 동기화 : <span style="color:#ff5a54">동시에 두개의 Thread가 동기화 블록 사용불가</span>

```java
class MyData {
    int data = 3;

    public synchronized void plusData() {
        int myData = data;
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        data++;
    }
}

class PlusThread extends Thread {
    MyData myData;

    // 생성자..
    public PlusThread(MyData myData) {
        this.myData = myData;
    }

    @Override
    public void run() {
        super.run();
        myData.plusData();
    }
}

class Main {
    public static void main(String[] args) {
        MyData myData = new MyData();   // 공유객체

        Thread plusThread1 = new PlusThread(myData);    // 각각의 객체가 공통된 Thread객체를 공유
        plusThread1.start();    // 4

        Thread plusThread2 = new PlusThread(myData);
        plusThread2.start();    // 5
    }
}
```

plusThread1이 완료된후 변화된 값이 유지되어 plusThread2의 값에도 진행.

<span style="color:#5aff54">Data Class</span> : synchronized를 data class의 적용하는 경우 해당 클래스의 필드의 값이 변경되면 나중에 해당 클래스의 객체를 새로 생성해서 사용해도 이전에 변경된 값이 그대로 유지됨.



```java
class MyData {
    synchronized void abc() {
        for(int i=0; i<3; i++) {
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    
    synchronized void bcd() {
        for (int i=0; i<3; i++) {
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    
    void cde() {
        synchronized (new Object()) {
            for (int i=0; i<3; i++) {
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

class Main {
    public static void main(String[] args) {
        MyData myData = new MyData();   // 공유객체
        
        new Thread() {
            @Override
            public void run() {
                myData.abc();   // abc()와 cde()는 동시에 실행
            }
        }.start();
        
        new Thread(){
            @Override
            public void run() {
                myData.bcd();   // abc()가 종료후 실행.
            }
        }.start();
        
        new Thread() {
            @Override
            public void run() {
                myData.cde();   // abc()와 동시에 실행됨.
            }
        }.start();
    }
}
```