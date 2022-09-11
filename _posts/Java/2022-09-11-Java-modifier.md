## 자바 제어자 (modifire)

### final

- final 필드, final 지역변수 : <span style="color:#FF5a54">값을 바꿀 수 없다.</span>

- ```java
  class A1{
      int a = 3;
      final int b = 5;
      A1() {
     
      }
  }
  
  class A2 {
      int a;
      final int b;
      A2() {
          a = 3;
          b = 5; 
      }
  }
  ```

  A1 클래스는 선언과 동시에 초기값을 지정해주었고 b는 이후에 값을 수정 할 수 없다.(상수 개념.)

  A2 클래스는 선언후 생성자에서 값을 지정해주었고 b 값은 생성자 이후에 값이 수정 될 수 없다.

  <span style="color:#FF5a54">A2 클래스의 경우 반드시 생성자에서는 값을 지정해주어야 한다.</span>

  

- final 메서드 : <span style="color:#FF5a54">상속시 Override 불가.</span>

  ```java
  class A {
      void abc() {}
      final void bcd() {}
  }
  
  class B extends A {
      void abc() {}
      // void bcd() {} 불가능..
  }
  ```

  

- final 클래스 : <span style="color:#FF5a54">상속 불가</span>

  ```java
  final class A{}
  
  class B extends A {} // 상속 불가능
  ```



### abstract

- abstract class : 추상 클래스, 추상 메서드를 하나 이상 가지고 있는 클래스.
  추상클래스를 상속받은 클래스는 강제로 추상 메서드를 오버라이드 해야 한다.
- abstract method : 추상 메서드, 해당 추상 클래스에서는 기능을 정의하지 않고 선언만 함.
  세부 기능은 상속받은 클래스에서 오버라이드하여 사용. <span style="color:#ff5a54">자식 클래스마다 상속받아 재정의가 필요한 메서드.</span>

<span style="color:#5aff54">Overload : 같은 이름의 메서드 이지만 파라미터의 개수, 자료형에 따라 정의 할 수 있는것</span>

Override vs Overload 구분하기.

