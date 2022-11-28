## Lambda

- 자바에서 <span style="color:#ff5a54">함수적 프로그래밍</span> 지원 기법
- 코드의 간결화 및 <span style="color:#ff5a54">병렬처리</span>에 강함(<span style="color:#ff5a54">Collection API 성능 효과적 개선</span> (Stream))

### 람다식 이해를 위한 <span style="color:#ff5a54">기본 용어</span>의 정리

- 함수(function) : 기능, 동작을 정의
- 메서드(method) : 클래스 또는 인터페이스 <span style="color:#ff5a54">내부에서 정의된 함수</span>
- 함수형 인터페이스(funtional interface) : 내부에 <span style="color:#ff5a54">단 1개의 추상메서드</span>만 존재하는 인터페이스

#### <span style="color:#ff5a54">본래의미의 함수적 프로그래밍</span>과 <span style="color:#ff5a54">객체지향형</span>의 개념적 비교

- <span style="color:#ff5a54">본래 의미의 함수적 프로그래밍</span>에서의 함수 사용 (자주 사용하는 기능 구현)
- <span style="color:#ff5a54">객체지향형 프로그래밍에서</span>의 메서드(함수=기능) 사용

자바는 새로운 함수 문법을 정의한 것이 아니라 <span style="color:#ff5a54">이미 있는 인터페이스를 빌어 람다식을 표현</span>
메서드의 정의 -> <span style="color:#ff5a54">람다식표현</span>
메서드의 호출 -> <span style="color:#ff5a54">참조변수, 메서드이름</span>

### <span style="color:#ff5a54">객체지향형과 자바의 함수적 프로그래밍</span>의 문법적 비교

```java
class Main {
    public static void main(String[] args) {
        // 객체 생성
        A a1 = new B();
        a1.abc();
        
        // 익명이너클래스
        A a2 = new A() {
            @Override
            public void abc() {
                
            }
        };
        
        // 람다식 표현
        A a3 = () -> {
            
        };
    }
}

class B implements A {
    @Override
    public void abc() {
        
    }
}

interface A {
    void abc();
}
```



### 람다식의 약식표현

```java
A a = () -> {}  // 람다식 표현
A a = () ->     // 실행문이 하나인 경우 중괄호 생략 가능
A a = (int a) -> {} // 매개변수가 있을때
A a = (a) -> {} // 매개변수 타입 생략가능
A a = a -> {}   // 매개변수가 한 개인 경우 () 생략
A a = (int a, int b) -> {return a+b;}
A a = (a, b) -> {return a+b;}   // 매개변수 타입 생략가능
A a = (a, b) -> a+b;    // 실행문으로 리턴만 있는 경우 return 생략 가능
```



### 람다식의 활용

- 익명이너클래스 내 구현 메서드의 약식(람다식) 표현(<span style="color:#ff5a54">함수적 인터페이스</span>만 가능)

```java
// 함수형 인터페이스
interface A {
    void method(int a);
}

// 익명 이너클래스 활용
A a = new A() {
    public void method(int a) {
        ...
    }
}

// 람다식 활용
A a = (int a) -> {...};
```

- 메서드 참조(<span style="color:#ff5a54">인스턴스 메서드 참조 Type1, 정적 메서드 참조, 인스턴스 메서드 참조 Type2</span>)

```java
interface A {
    void abc();
}

class B {
    static void bcd() {
        ...
    }
}

A a = new A() {
    @Override
    public void abc() {
        B.bcd();
    }
}

A a = () -> {
    B.bcd();
}
```

- 생성자 참조(<span style="color:#ff5a54">배열 생성자 참조, 클래스 생성자 참조</span>)

-> <span style="color:#ff5a54">배열</span>의 <span style="color:#ff5a54">new 생성자</span>를 참조하는 경우
-> interface 메서드의 <span style="color:#ff5a54">리턴타입 = 배열객체</span>

```java
interface A {
    int[] abc(int len);
}

A a = new A() {
    @Override
    public int[] abc(int len) {
        return new int[len];
    }
}

A a = (len) -> new int[len];

A a = int[]::new;
```

<span style="color:#ff5a54">배열 생성자 참조</span>를 위해서는 인터페이스 메서드의 매개변수로 배열의 길이를 전달

```java
interface A {
    B abc(int k);
}

class B {
    B() {
        //첫번째 생성자
    }
    B(int k){
        //두번째 생성자
    }
}

A a = new A() {
    B abc(int k){
        return new B(k);
    }
}

A a = (k)-> {
    new B(k);
}

A a = B::new;
```

<span style="color:#ff5a54">클래스 생성자 참조</span>를 위해서는 인터페이스 메서드의 <span style="color:#ff5a54">매개변수에 따라 생성자 선택</span>

