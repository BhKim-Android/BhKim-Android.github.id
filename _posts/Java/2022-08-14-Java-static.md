## 자바제어자

### static -> 객체생성 없이 바로 사용 가능.

- Static 필드는 힙메모리 객체에는 저장공간이 없음
- 저장공간은 static 영역에 있다.

### static 메서드에서 사용가능한 필드 및 메서드

- Static 메서드내에서는 static 멤버만 사용 가능함(<span style="color:#FF5A54">객체 생성이전에 사용가능해야 하기 때문</span>)
- 정적메서드 내부에서는 클래스 객체 참조변수인 this 키워드를 사용할 수 없음
- This 객체를 사용할 수 없으면 객체를 통해서만 접근할 수 있는 <span style="color:#FF5A54">인스턴스 멤버를 사용할 수 없음</span>
- 인스턴스 메서드 내부 : 인스턴스 멤버 + 정적 멤버 모두 사용가능.
- 정적 메서드 내부 : 정적 멤버만 사용 가능.

### static 초기화 블록

- instance 필드의 초기화 위치 -> 생성자
- Static 필드의 초기화 위치 -> static {} (생성자 호출없이 사용가능하기 위해.)

### JVM에서의 main 메서드 실행

### ![스크린샷 2022-08-15 오전 2.54.55](/Users/byunghoonkim/Library/Application Support/typora-user-images/스크린샷 2022-08-15 오전 2.54.55.png)

### kotlin과 static

```kotlin
public static final String REQUEST = "REQUEST";
public static void kimbh() {
    // Do something
}
```

kotlin에서 상수(정적)를 사용하거나 정적 메서드를 사용할때 static키워드를 사용한다.

```kotlin
const val REQUEST = "REQUEST"
fun kimbh() {
    // Do something
}
```
