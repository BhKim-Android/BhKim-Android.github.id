## super와 super()

### super 키워드 vs super() 메서드

- Super 키워드 - > 부모클래스의 객체.
- 필드명 중복 or 메서드 오버라이딩으로 가려진 부모의 필드/메서드를 호출하기 위해 주로 사용

```kotlin
class A {
    fun a() {
        Log.d("TAG", "A Class a()메서드")
    }
}

class B : A() {
    override fun a() {
        // 오버라이딩..
        Log.d("TAG", "B Class a()메서드")
    }

    fun b() {
        a() // this.a()
    }
    
    fun instance() {
        val bb = B()
        bb.b()  // B Class a()메서드
    }
}
```

- This 키워드는 지금 class를 참조.

```kotlin
class A {
    fun a() {
        Log.d("TAG", "A Class a()메서드")
    }
}

class B : A() {
    override fun a() {
        // 오버라이딩..
        Log.d("TAG", "B Class a()메서드")
    }

    fun b() {
        super.a()
    }

    fun instance() {
        val bb = B()
        bb.b()  // A Class a()메서드
    }
}
```

- Super 키워드는 상속받은 부모 class를 참조.

### super() 메서드 : 부모 클래스의 생서자를 호출 -> <span style="color:#FF5a54">자식 클래스 객체 속에 부모 객체가 포함될 수 있었던 이유.</span>

- super() 메서드는 생성자 내부에서만 사용 가능.
- 반드시 중괄호 이후 첫줄에 위치 하여야 함.
- 자식클래스 생성자의 첫줄에는 반드시 this() or super()가 포함 되어야함.
  (생략시 컴파일러가 자동으로 super() 추가)

