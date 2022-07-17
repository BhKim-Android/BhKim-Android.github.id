## 연산자

![스크린샷 2022-07-17 오후 10.37.18](/Users/byunghoonkim/Library/Application Support/typora-user-images/스크린샷 2022-07-17 오후 10.37.18.png)

1. 산술연산자
   기본적인 4칙연산을 하는 연산자.(특징. 나머지를 return 하는 연산자 '%')

2. 증감연산자
   전위형 ++a, 후위형 a++ 전위형은 이미 연산인 완료된, 후위형은 해당 라인이 모두 처리된후 변수에 증감된 value가 대입됨.

3. 비트연산자(&, |,~, ^) <- 2진수 연산자
   & : AND연산 비교하는 두값이 모두 1일때만 1을 반환
   | : OR연산 비교하는 두값중 하나가 1이면 1을 반환
   ^ : XOR연산 두값이 다를때만 1을 반환
   ~ : NOT연산 특정 값을 1에서 0으로 0에서 1로 바꾸는 연산자.

   ![스크린샷 2022-07-17 오후 10.44.59](/Users/byunghoonkim/Library/Application Support/typora-user-images/스크린샷 2022-07-17 오후 10.44.59.png)

```연산
val data: Int = 10

Integer.toBinaryString(data) // 1010
Integer.toOctalString(data) //12
Integer.toHexString(data) //a

Integer.parseInt("1010", 2) //10 : 2진수 -> 10진수
Integer.parseInt("12", 8) //10 : 8진수 -> 10진수
Integer.parseInt("a", 16) //10 : 16진수 -> 10진수
```

#### NOT 연산

양수 읽는법 : 1을 기준으로 값을 읽음
음수 읽는법 : 0을 기준으로 값을 읽음

![스크린샷 2022-07-17 오후 11.12.39](/Users/byunghoonkim/Library/Application Support/typora-user-images/스크린샷 2022-07-17 오후 11.12.39.png)

#### 쉬프트 연산자 (<<, >>, >>>)

![스크린샷 2022-07-17 오후 11.13.49](/Users/byunghoonkim/Library/Application Support/typora-user-images/스크린샷 2022-07-17 오후 11.13.49.png)

논리쉬프트(>>>) : 부호비트 상관없이 쉬프트 수행.

### 비교연산자

- 크기비교(>, <, >=, <=) 연산 결과를 boolean으로 return
- 등가비교(==, !=) 

### 논리연산자

- 논리 AND 연산자(&&) : 두 조건이 true일때 true를 return
- 논리 OR 연산자(||) : 두 조건중 하나가 true이면 true를 return 
- 비트 논리 연산자(&, |) : 결과는 논리 연산자와 동일하다 그러나 논리 연산자 보다 속도가 빠르다 <-Short Circuit

### 대입연산자

- 대입연산자 (=) : 오른쪽의 연산 결과를 왼쪽의 변수에 대입

  ![스크린샷 2022-07-17 오후 11.23.47](/Users/byunghoonkim/Library/Application Support/typora-user-images/스크린샷 2022-07-17 오후 11.23.47.png)

### 삼항연산자

![스크린샷 2022-07-17 오후 11.24.55](/Users/byunghoonkim/Library/Application Support/typora-user-images/스크린샷 2022-07-17 오후 11.24.55.png)

Kotlin의 ?: 와는 완전히 다른 연산.
?: -> value가 null인경우 대입할 예외 value를 지정하는 방식.
