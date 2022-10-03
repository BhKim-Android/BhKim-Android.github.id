## 이너클래스(inner class)

### 클래스 내부에 정의된 클래스

- 멤버 클래스 : 클래스 내부에 정의된 클래스 (Kotlin은 class 앞에 inner를 붙임.)
  1. 일반 클래스
  2. 정적(static) 클래스
- 지역 클래스 : 메서드 내부에 정의된 클래스.

### 인터페이스 멤버 이너클래스

- 특징 : 외부클래스의 <span style="color:#ff5a54">모든 접근지정자의 멤버 접근</span> 가능
- 생성클래스명 : A.class, A$B.class (외부클래스.class ,  외부클래스$내부클래스.class)

#### 이너클래스 객채 생성

- 외부클래스 객체 생성 
  A a = new A();
- 내부클래스 객체 생성
  A.B b = A.new B();

#### 인스턴스 멤버 이너클래스

```java
public class JavaClass {
    public int a = 3;
    protected int b = 4;
    int c = 5;
    private int d = 6;
    
    void abc() {
        Log.d("Tag", "메서드 호출");
    }
    
    class InnerJavaClass {
        void bcd() {
            a = 4;
            b = 3;
            c = 2;
            d = 1;
            abc();
        }
    }
}
```

이너클래스에서 외부클래스의 멤버들을 참조할때 모든 접근제어자에 접근이 가능하다.

```java
public class JavaClass {
    int a = 3;
    int b = 4;
    
    class InnerJavaClass{
        int a = 5;
        int b = 6;
        
        void bcd() {
            Log.d("Tag", a); // 5
            Log.d("Tag", b); // 6
            Log.d("Tag", JavaClass.this.a); // 3
            Log.d("Tag", JavaClass.this.b); // 4
        }
    }
}
```

인스턴스 멤버이너클래스는 힙메모리내의 외부클래스 객체내에 생성되기 때문에 외부클래스 객체를 먼저 생성 하여야 한다.

#### 정적 멤버 이너클래스

- 특징 : 외부클래스의 static 멤버만 접근가능

```java
public class JavaClass {
    int a = 3;
    static int b = 4;

    static class InnerJavaClass{
        void bcd() {
            System.out.println(a);  // 참조 불가.
            System.out.println(b);  // 참조 가능.
        }
    }
}
```



#### 지역 이너클래스

- 메서드 내부에서 정의된 클래스
- 외부클래스의 필드는 모두 접근 가능
- 메서드 지역변수는 <span style="color:#ff5a54">final만 사용가능</span>

생성 클래스명 : A.class, A$1B.class

```java
public class JavaClass {
    int a = 3;

    void abc() {
        int b = 5;
        
        class InnerJavaClass {
            void bcd() {
                System.out.println(a);  // 필드 (o).
                System.out.println(b);  // 지역변수 (o).
                a = 7;  // 필드값 변경(o)
                b = 7;  // 지역변수값 변경(x)
            }
        }
        
        InnerJavaClass innerJavaClass = new InnerJavaClass();
        innerJavaClass.bcd();
    }
}
```

```java
public class A {
    void abc() {
        class B{}
        class C{}
    }
    
    void bcd() {
        class C{}
        class D{}
    }
}
```

각 클래스명.

A.class
A$1B.class
A$1C.class
A$2C.class
A$1D.class
