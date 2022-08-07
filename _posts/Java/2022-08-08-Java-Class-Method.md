## 메서드

- 리번 타입 : 메서드가 동작후 반환되는 값에 대한 자료형.
- 반환값이 null 인경우 void로 선언.
- 반환값이 지정된경우 메서드의 마지막에는 반드시 return 으로 반환할 값을 리턴.
- 입력매개변수 : 파라미터.(메서드 내부에서 사용할 매개변수.)

### 메서드의 호출.

- 메서드는 기본적으로 클래스 내부에서 호출하여 사용.
- 다른 클래스의 메서드는 객체를 생성하여 호출이 가능하다(But. private는 불가.)

```kotlin
class Sample {
    var a = 0
    private var b = "Test"

    fun publicMethod(): String {
        return b
    }

    private fun privateMethod(): Int {
        return a
    }
}

class Main {
    fun main() {
        val sample = Sample() // Sample 클래스 객체 생성.
        
        sample.publicMethod() // b는 pivate지만 public method를 통하여 "Test"값을 리턴.
        sample.privateMethod() // private는 호출이 불가하여 오류 발생.
    }
}
```



### 오버로딩 (Overloading)

- 컴파일러는 메서드 시그너처가 다르면 메서드 이름이 동일하여도 다른 메서드로 인식.
- 즉 같은 이름의 메서드도 입력매개변수가 다르면 하나의 클래스에서 여러번 정의가 가능.
