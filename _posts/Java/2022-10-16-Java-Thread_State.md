## Thread 상태 (State)

- New : Thread의 <span style="color:#ff5a54">객체 생성.</span>
  Thread myThread = new MyThread()

- Runnable : Thread가 CPU를 사용하는 <span style="color:#ff5a54">최소 단위</span> (실행대기와 실행 둘다 Runnable 상태.)
  myThread.start() <- Runnable 상태의 시작

- Terminated : run() 완료된 상태.

- <span style="color:#ff5a54">Tiemd Waiting</span> : 일정시간동안 Thread를 정지. (<span style="color:#ff5a54">interrupt()</span>는 설정한 일정 시간이 지나기 전에 예외가 발생하여 catch가 강제 실행됨.)
  Thread.sleep(time) / (객체).join(time)

- Blocked : 두개의 thread가 동기화 block을 사용하는 경우 lock이걸려 작업을 할 수 없는 Thread의 상태

- Waiting : 두개의 Thread가 동시에 실행 될 수 없을때 하나의 Thread를 정지시키는 상태
  Join(), wait()

  

### Thread의 상태값 가져오기 (Thread 클래스의 인스턴스 메서드)

Thread.State getState() -> Thread의 상태값을 enum 타입인 Thread.State 객체로 리턴.
Ex) BLOCKED, NEW, RUNNABLE, TERMINATED, TIMED_WAITING, WATING

```java
class Main {
    public static void main(String[] args) {
        Thread.State state;
        
        Thread thread = new Thread() {
            @Override
            public void run() {
                for(long i =0; i<10000000L; i++){}
            }
        };
        state = thread.getState();  // NEW
        
        thread.start(); // RUNNABLE

        try {
            thread.join();  // TERMINATED
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```



### YieldFlag

```java
class Main {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        thread1.yieldFlag = false;
        thread1.setDaemon(true);
        thread1.start();
        
        MyThread thread2 = new MyThread();
        thread2.yieldFlag = true;
        thread2.setDaemon(true);
        thread2.start();
        
        for(int i=0; i<6; i++) {
            try {
                Thread.sleep(1000); // 1초마다 한번씩 양보.
            } catch (InterruptedException e) {
                thread1.yieldFlag = !thread1.yieldFlag;
                thread2.yieldFlag = !thread2.yieldFlag;
            }
        }
    }
}

class MyThread extends Thread {
    boolean yieldFlag;

    @Override
    public void run() {
        while (true) {
            if (yieldFlag) {
                Thread.yield();
            } else {
                for (long i=0; i<1000000L; i++) {}
            }
        }
    }
}
```

- 다른 쓰레드에게 <span style="color:#ff5a54">CPU 사용을 양보</span>하고 자신은 실행대기 상태로 전환.



### BLOCKED

```java
class Main {
    public static void main(String[] args) {
        MyBlockTest mbt = new MyBlockTest();
        mbt.startAll();
    }
}

class MyBlockTest {
    MyClass mc = new MyClass(); // 공유객체
    
    Thread t1 = new Thread("thread1") {
        @Override
        public void run() {
            mc.syncMethod();
        }
    };

    Thread t2 = new Thread("thread2") {
        @Override
        public void run() {
            mc.syncMethod();
        }
    };

    Thread t3 = new Thread("thread3") {
        @Override
        public void run() {
            mc.syncMethod();
        }
    };
    
    void startAll() {
        t1.start();
        t2.start();
        t3.start();
    }
    
    class MyClass {
        synchronized void syncMethod() {
            for (long i =0; i<100000000L; i++) {}
        }
    }
}
```

1. thread1 -> Runnable
   thread2 -> Blocked
   thread3 -> Blocked
2. Thread1 -> Terminated
   Thread2 -> Blocked
   Thread3 -> Runnable
3. Thread1 -> Terminated
   thread2 -> Runnable
   Thread3 -> Terminated



### Waiting

```java
class DataBox {
    int data;

    synchronized void inputData(int data) {
        this.data = data;
        System.out.println("입력 데이터: " + data);
    }
    
    synchronized void outputData() {
        System.out.println("출력 데이터: " + data);
    }
}

class Main {
    public static void main(String[] args) {
        DataBox dataBox = new DataBox();
        Thread t1 = new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    dataBox.inputData(i);
                }
            }
        };
        
        Thread t2 = new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 10; i++) {
                    dataBox.outputData();	// 동기화
                }
            }
        };
        t1.start();
        t2.start();
    }
}
```

출력값

입력데이터 : 0
입력데이터 : 1
입력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
출력데이터 : 2
입력데이터 : 4
입력데이터 : 5
입력데이터 : 6
입력데이터 : 7
입력데이터 : 8
입력데이터 : 9

```java
class DataBox {
    boolean isEmpty = true;
    int data;

    synchronized void inputData(int data) throws InterruptedException {
        if (!isEmpty) {
            wait();
        }
        isEmpty = false;
        this.data = data;
        System.out.println("입력 데이터: " + data);
        notify();
    }

    synchronized void outputData() throws InterruptedException {
        if (isEmpty) {
            wait();
        }
        isEmpty = true;
        System.out.println("출력 데이터: " + data);
        notify();
    }
}
```

입력데이터: 0
출력데이터: 0
입력데이터: 1
출력데이터: 1
입력데이터: 2
출력데이터: 2
입력데이터: 3
출력데이터: 3
...
입력데이터: 9
출력데이터: 9
