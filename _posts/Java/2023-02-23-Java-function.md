

## 자바 API의 함수적 인터페이스

### 메서드의 매개변수에 사용되는 함수적 인터페이스

<img src = "https://user-images.githubusercontent.com/82013205/220827681-4226e5ee-e272-42d5-9051-3a7291ba04a1.png">

<img src ="https://user-images.githubusercontent.com/82013205/220827993-9dfb1910-38a9-4971-bf9a-c2c244f6ef38.png">

XXXOperator는 입력 매개변수의 연산 결과는 동일한 타입으로 리턴하는 기능
<span style="color:#ff5a54">UnaryOperatore : (입력 T -> 출력 T)</span>
<span style="color:#ff5a54">BinaryOperator : (입력 (T, T) -> 출력 T)</span>

```java
@FunctionalInterface
public interface UnaryOperatore<T> extends Function<T, T> { }

@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T, T, T>{ }
```



### <span style="color:#ff5a54">표준형</span> Consumer<T> 함수적 인터페이스

```java
interface Consumer<T> {
    public abstract void accept(T t);
}
```

Consumer<T> -> 입력타입(T) -> 출력없음(void)



#### 익명이너클래스

```java
Consumer<String> c = new Consumer<String>() {
    @Override
    public void accept(String t) {

    }
};

c.accept("Consumer<T> 함수형 인터페이스");
```

#### 표준형 람다식

```java
Consumer<String> c = str -> { };

c.accept("Consumer<T> 함수형 인터페이스");
```



### <span style="color:#ff5a54">확장형</span> Consumer<T> 함수적 인터페이스

|                                                     | 입력타입 | 출력 |
| --------------------------------------------------- | -------- | ---- |
| <span style="color:#ff5a54">Int</span>Consumer      | int      | void |
| <span style="color:#ff5a54">Long</span>Consumer     | long     | void |
| <span style="color:#ff5a54">Double</span>Consumer   | double   | void |
| <span style="color:#ff5a54">Bi</span>Consumer<T, U> | T, U     | void |

리턴타입이 <span style="color:#ff5a54">기본자료형이 아니거나</span> 확장형 인터페이스의 <span style="color:#ff5a54">리턴타입의 모두 동일한 경우</span> 인터페이스 <span style="color:#ff5a54">메서드명은 동일</span>함 (확장형 Consumer<T>는 모두 accept(..))



```java
IntConsumer c2 = num -> System.out.println("int 값 = " + num);
LongConsumer c3 = num -> System.out.println("Long 값 = " + num);
DoubleConsumer c4 = num -> System.out.println("Double 값 = " + num);
BiConsumer<String, Integer> c5 = (name, age) -> System.out.println("이름:" + name + " 나이:"+age);

c2.accept(3);
c3.accept(5L);
c4.accept(7.8);
c5.accept("홍길동", 16);
```



### <span style="color:#ff5a54">표준형</span> Supplier<T> 함수적 인터페이스

Supplier<T> -> 입력없음(void) -> 출력타입(T)

```java
interface Supplier<T> {
    public abstract T get();
}
```

#### 익명이너클래스

```java
Supplier<String> s = new Supplier<String>() {
    @Override
    public String get() {
        return "Supplier<T> 함수형 인터페이스";
    }
};
```



#### 표준형 람다식

```java
Supplier<String> s = () -> "Supplier<T> 함수형 인터페이스";
```

|                                                    | 입력타입 | 출력타입                                                    |
| -------------------------------------------------- | -------- | ----------------------------------------------------------- |
| <span style="color:#ff5a54">Boolean</span>Supplier | 입력없음 | Boolean (getAs<span style="color:#ff5a54">Boolean</span>()) |
| <span style="color:#ff5a54">Int</span>Supplier     | 입력없음 | int (getAs<span style="color:#ff5a54">Int</span>())         |
| <span style="color:#ff5a54">Long</span>Supplier    | 입력없음 | Long (getAs<span style="color:#ff5a54">Long</span>())       |
| <span style="color:#ff5a54">Double</span>Supplier  | 입력없음 | Double (getAs<span style="color:#ff5a54">Double</span>())   |

리턴타입이 <span style="color:#ff5a54">기본자료형이면서</span> 확장형 인터페이스의 <span style="color:#ff5a54">리턴타입이 다른 경우</span> 인터페이스 <span style="color:#ff5a54">메서드명은 서로 다름</span> (<span style="color:#ff5a54">기본메서드명As리턴타입명</span>)

```java
BooleanSupplier s2 = () -> false;
IntSupplier s3 = () -> 2+3;
LongSupplier s4 = () -> 10L;
DoubleSupplier s5 = () -> 5.8;

s2.getAsBoolean();  // false
s3.getAsInt();  // 5
s4.getAsLong(); // 10
s5.getAsDouble();   // 5.8
```



### <span style="color:#ff5a54">표준형</span> Predicate<T> 함수적 인터페이스

Predicate<T> -> 입력타입(T) -> 출력타입(boolean)

```java
interface Predicate<T> {
    public abstract boolean test(T t);
}
```

#### 익명이너클래스

```java
Predicate<String> p = new Predicate<String>() {
    @Override
    public boolean test(String s) {
        return (s.length() > 1) ? true : false;
    }
};
```

#### 표준형 람다식

```java
Predicate<String> p = s -> s.length() > 1;
```



|                                                    | 입력타입 | 출력타입 |
| -------------------------------------------------- | -------- | -------- |
| <span style="color:#ff5a54">Int</span>Predicate    | int      | Boolean  |
| <span style="color:#ff5a54">Long</span>Predicate   | long     | Boolean  |
| <span style="color:#ff5a54">Double</span>Predicate | double   | boolean  |
| <span style="color:#ff5a54">Bi</span>Predicate     | T, U     | boolean  |

리턴타입이 <span style="color:#ff5a54">기본자료형이 아니거나</span> 확장형 인터페이스의 <span style="color:#ff5a54">리턴타입이 모두 동일한 경우</span> 인터페이스 <span style="color:#ff5a54">메서드명은 동일</span>함 (확장형 Predicate<T>는 모두 test(..))

```java
IntPredicate p2 = num -> num % 2 == 0;
LongPredicate p3 = num -> num > 100;
DoublePredicate p4 = num -> num > 0;
BiPredicate<String, String > p5 = (str1, str2) -> str1.equals(str2);

p2.test(2); // true
p3.test(85L);   // false
p4.test(-5.8);  // false
p5.test("안녕", "안녕");    // true
```



### <span style="color:#ff5a54">표준형</span> Function<T, R> 함수적 인터페이스

Function<T, R> -> 입력타입(T) -> 출력타입(R)

```java
interface Function<T, R> {
    public abstract R apply (T t);
}
```

#### 익명이너클래스

```java
Function<String, Integer> f = new Function<String, Integer>() {
    @Override
    public Integer apply(String s) {
        return s.length();
    }
};
```

#### 표준형 람다식

```java
Function<String, Integer> f = s -> s.length();
```



|                                                        | 입력타입 | 출력타입 |
| ------------------------------------------------------ | -------- | -------- |
| <span style="color:#ff5a54">Int</span>Function<R>      | int      | R        |
| <span style="color:#ff5a54">Long</span>Function<R>     | long     | R        |
| <span style="color:#ff5a54">Double</span>Function<R>   | double   | R        |
| <span style="color:#ff5a54">Bi</span>Function<T, U, R> | T, U     | R        |

리턴타입이 <span style="color:#ff5a54">기본자료형이 아니거나</span> 확장형 인터페이스의 <span style="color:#ff5a54">리턴타입이 모두 동일한 경우</span> 인터페이스 <span style="color:#ff5a54">메서드명은 동일</span>함 (이 경우 모두 apply(..))



```java
IntFunction<Double> f2 = num -> (double) num;   // int -> double
LongFunction<String> f3 = num -> String.valueOf(num);   // long -> 문자열
DoubleFunction<Integer> f4 = num -> (int) num;  // double -> int
BiFunction<String, Integer, String > f5 = (name, age) -> "이름: " + name + ", 나이:" + age;

f2.apply(10);   //10.0
f3.apply(20L);  // 20
f4.apply(30.5); // 30
f5.apply("홍길동", 16);    // 이름: 홍길동, 나이:16
```



### <span style="color:#ff5a54">확장형</span> Function<T, R> 함수적 인터페이스

|                                                        | 입력타입 | 출력타입                                                    |
| ------------------------------------------------------ | -------- | ----------------------------------------------------------- |
| <span style="color:#ff5a54">ToInt</span>Function<T>    | T        | Int (applyAs<span style="color:#ff5a54">Int</span>())       |
| <span style="color:#ff5a54">ToLong</span>Function<T>   | T        | long (applyAs<span style="color:#ff5a54">Long</span>())     |
| <span style="color:#ff5a54">ToDouble</span>Function<T> | T        | double (applyAs<span style="color:#ff5a54">Double</span>()) |

```java
ToIntFunction<String> f6 = str -> str.length(); // str -> int
ToLongFunction<Double> f7 = num -> num.longValue(); // double -> long
ToDoubleFunction<Integer> f8 = num -> num/100.0;    // int -> double

f6.applyAsInt("반갑습니다.");    //6
f7.applyAsLong(58.254);     // 58
f8.applyAsDouble(250);      // 2.5
```