## 자바 제어자

### 접근제어자

- <span style="color:#FF5A54">public</span> : 접근 제한이 없다.
- <span style="color:#FF5A54">protected</span> : 같은 패키지 내 그리고 다른 패키지의 자식 클래스에서 접근 가능.
- <span style="color:#FF5A54">default</span> : 같은 패키지 내에서만 접근 가능
- <span style="color:#FF5A54">private</span> : 같은 클래스 내에서만 접근 가능

### 사용 가능 접근제어자

- Class : public, default
- 메서드, 멤버변수 : public, protected, (default), private



### <span style="color:#5AFF54">Kotlin 접근제어자</span>

- <span style="color:#FF5A54">public</span> : protected + 접근제한 없음.
- <span style="color:#FF5A54">protected</span> : private + 상속받은 클래스에서도 접근가능.
- <span style="color:#FF5A54">internal</span> : private + 같은 모듈 안에서 접근가능.
- <span style="color:#FF5A54">private</span> : 속해있는 class 에서만 접근 가능.

```kotlin
open class OuterK {
    private val a = 1
    private class Inner {
        val e = 5
    }
}

class SubClass : OuterK() {
    init {
        val a = this.a // access 불가
        val e = Inner().e // access 불가
    }
}

class Unrelated(o: OuterK) {
    init {
        val a = o.a // access 불가
        val e = OuterK.Inner().e // access 불가
    }
}
```

OuterK 클래스에 private 가 붙은 변수나 클래스 모두 같은 패키지라도 접근할 수 없다.



```kotlin
open class OuterK {
    protected open val b = 2
    protected class Inner2 {
        val f = 6
    }
}

class SubClass : OuterK() {
    override val b = 5 // access 가능
    init {
        val b = this.b // access 가능
        val f = Inner2().f // access 가능
    }
}

class Unrelated(o: OuterK) {
    init {
        val b = o.b // access 불가
        val f = OuterK.Inner2().f // access 불가
    }
}
```

OuterK 클래스에 protected 가 붙어있는 변수나 클래스는 동일한 class 내에서 뿐만아니라 상속받은 class 에서도 접근이 가능하지만 다른 class 에서는 접근할 수 없다.



```kotlin
open class OuterK {
    internal val c = 3
    internal class Inner3 {
        val g = 7
    }
}

class SubClass : OuterK() {
    init {
        val c = this.c // access 가능
        val g = Inner3().g // access 가능
    }
}

class Unrelated(o: OuterK) {
    init {
        val c = o.c // access 가능
        val g = OuterK.Inner3().g // access 가능
    }
}
```

OuterK 클래스에 internal 가 붙어있는 변수나 클래스는 동일한 class 나 다른 class 내에서 뿐만아니라 상속받은 class 에서도 접근이 가능하다
