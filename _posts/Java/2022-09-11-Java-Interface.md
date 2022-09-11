## 인터페이스(Interface)

### Interface?

- 모든 필드가 public <span style="color:#FF5a54">static final</span>로 정의 (static : 객체를 생성하지 않아도 참조가 가능, final : 불변(상수).)
- 모드 메서드가 abstract로 정의 (default, static 메서드 제외)
- 디폴트 메서드는 public으로 정의
- 자체적인 객체 생성 불가. <span style="color:#ff5a54">익명 이너 클래스 </span>



### Kotlin vs Java Interface

```java
interface Java {
    public static final int a = 3;
    // public abstract void abc();
    default void abc() {}
    static void def() {}
}
```

- 인터페이스 에서는 추상메서드만 사용이 가능하지만 Java 1.8 이후로 default 메서드가 추가됨.

```kotlin
interface Kotlin {
    companion object {
        const val a = 3
    		fun def() {}
    }
    fun abc() {}
}
```

- kotlin에선 default 메서드를 일반 메서드처럼 사용. static은 companion을 사용해야함.



### default Method

- 의미 : 인터페이스 내부의 완료된 메서드.
- 필요 이유 : 인터페이스에 기능(메서드)을 추가할때 이전에 상속받은 클래스들이 해당 메서드를 전부 상속받아야 하는 이슈가 있는데 디폴트 메서드로 만들면 필요한 클래스들만 해당 메서드를 상속받아 사용하면 되는 장점이 생김.

### static Method

- 의미 : 클래스 내부의 정적 메서드와 사용방법과 동일 (객체 생성 없이 클래스 이름으로 바로 접근 가능.)



