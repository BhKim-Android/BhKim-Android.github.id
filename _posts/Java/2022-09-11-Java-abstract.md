## Abstract Class & Interface

- Class : 객체 생성이 가능함. <span style="color:#FF5a54">상속받은 필드를 제외하고 일반적인 메서드만 정의가 가능</span> 클래스/인터페이스 상속 가능/
- abstract Class : 객체 생성이 불가능함. <span style="color:#FF5a54">추상 메서드와 일반 메서드 정의 가능.</span> 클래스/인터페이스 상속 가능.
- Interface : 객체 생성이 불가능함. <span style="color:#FF5a54">추상 메서드만 정의 가능.</span> 클래스/인터페이스 상속 불가능.

### 추상 클래스

- 익명 이너 클래스 : 추상 클래스는 객체화 할 수 없지만 익명 이너 클래스를 정의하여 <span style="color:#ff5a54">가상의 클래스를 만들어 해당 추상클래스를 상속받는 동작을 한다.</span>

```java
abstract class A {
    abstract  void abc();
}

A a = new A() {
    @Override
    void abc() {
        
    }
}
```

Android의 각종 이벤트 인터페이스를 생각하면됨.
익명의 Class x 를 생성후 해당 클래스가 임시로 추상 클래스or인터페이스를 상속받고 일시적으로 객체화됨.

추상 클래스가 다중 상속이 아닌경우는 익명이너클래스가 코드가 간결하다.
