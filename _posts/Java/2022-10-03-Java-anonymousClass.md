## 익명이너클래스 (Anonymous class)

```java
public class A {
    C b = new B();  // 필드
    
    void abc() {    // 메서드
        b.bcd();
    }
    
    class B implements C {  // 이너클래스
        @Override
        public void bcd() {}
    }
}

interface C {
    public abstract void bcd();
}
```

C 인터페이스를 상속받아 bcd() 메서드를 오버라이딩함.
익명이너클래스를 사용하는 경우 이름이 없어 한번에 객체를 <span style="color:#ff5a54">2개 이상 생성 불가능</span>

```java
interface C {
    public abstract void bcd();
}

class Main {
    C c = new C() {
        @Override
        public void bcd() {
            cde();  // 내부적으로 호출 가능.
        }
        void cde() {}
    };
    
    void abc() {
        B b = new B();
        b.bcd();
        b.cde();
        
        c.bcd();
        c.cde();    // (x) // C 타입에 없기때문
    }
}

class B implements C {
    @Override
    public void bcd() {}
    void cde() {}
}
```



#### 익명이너클래스를 활용한 인터페이스 타입의 매개변수 전달

```java
interface A {
    public abstract void abc();
}

class C {
    void cde(A a) { // 매개변수로 인터페이스 타입의 변수를 전달..
        a.abc();
    }
}

class B implements A {
    @Override
    public void abc() {}
}

class Main{
    C c = new C();
    A a1 = new B();
    void main() {
        c.cde(a1);	// A를 상속받은 B의 인스턴스인 a1을 파라미터로 전달..
        c.cde(new B()); // 동일..
    }
}
```

```java
interface A {
    public abstract void abc();
}

class C {
    void cde(A a) { // 매개변수로 인터페이스 타입의 변수를 전달..
        a.abc();
    }
}

class Main{
    C c = new C();
    void main() {
        c.cde(new A() {	// 매개변수에 익명의 이너클래스를 만들어서 전달.
            @Override
            public void abc() {
                
            }
        });
    }
}
```



### 이너인터페이스

#### 내부 인터페이스

- 외부 클래스와 밀접한 관계가 있는 경우에 정의
- <span style="color:#ff5a54">UI의 이벤트 처리에 가장 많이 사용</span>(클릭, 터치 등)
- static을 생략한 경우 <span style="color:#ff5a54">컴파일러는 자동으로 static 삽입</span>

이너인터페이스 구현 클래스 생성 익명이너클래스로 객체 생성.

```java
class A {
    interface B {
        public abstract void bcd();
    }
}

class C implements A.B {
    @Override
    public void bcd() {}
}

class Main {
    void main() {
        // 클래스로 객체 생성.
        C c = new C();
        c.bcd();
        
        // 익명이너클래스로 객체 생성.
        A.B b = new A.B() {
            @Override
            public void bcd() {
                
            }
        };
        b.bcd();
    }
}
```
