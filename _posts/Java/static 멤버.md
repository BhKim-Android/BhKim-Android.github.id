## 필드와 static 멤버(필드/메서드)의 중복

### <span style="color:#ff5a54">인스턴스 필드</span> 오버라이딩

- 인스턴스 필드는 오버라딩이 되지 않음.

```kotlin
class A {
    val m = 3
}

class B : A() {
    fun b() {
        Log.d("TAG", "$m")  // 참조 불가.
        val a = A()
        a.m // 참조가능
    }
}
```

### static을 포함한 const val 도 오버라이딩은 불가능.



### 인스턴스 멤버/static 멤버 오버라이딩 여부 정리.

- 인스턴스 필드 : 오버라이딩(x)
- 인스턴스 메서드 : 오버라이딩(o)
- static 필드 : 오버라이딩(x)
- <span style="color:#ff5a54">static 메서드</span> : 오버라이딩(x)