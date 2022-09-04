## 최상위 클래스 Object(객체)

### Object 클래스 : 모든 자바 클래스의 부모 클래스

- 상속받은 클래스가 별로도 표기되지 않은 경우 Object 클래스를 상속받음(생략)

String : toString() -> Object 객체의 정보 <span style="color:#FF5a54">패키지.클래스명@해쉬코드</span> 일반적으로 오버라이딩해서 사용.

Boolean : equals(obj: Object) -> 매개변수 obj 객체와 stack메모리 값(번지) 비교 즉, 비교연산자 ==와 동일한 결과

Int : hashCode() -> 객체의 hashCode()값 리턴. hashTable, hashMap 등의 <span style="color:#FF5a54">동등비교</span>에 사용 위치값을 기반으로 생성된 <span style="color:#FF5a54">고유값</span>



### Object 메서드 : hashcode()

- 객체의 hashCode()값 리턴. hashTable, HashMap 등의 동등비교에 사용

참고. HashMap -> 하나의 데이터는 key와 value 쌍으로 구성.

```kotlin
class A constructor(val name: String) {

    override fun equals(other: Any?): Boolean {
        if (other is A) {
            if (this.name == (other as A).name) {
                return true
            }
        }
        return false
    }

    override fun toString(): String {
        return name
    }
}

class B constructor(val name: String) {
    override fun equals(other: Any?): Boolean {
        if (other is B) {
            if (this.name == (other as B).name) {
                return true
            }
        }
        return false
    }

    override fun hashCode(): Int {
        return name.hashCode()
    }

    override fun toString(): String {
        return name
    }
}
```

