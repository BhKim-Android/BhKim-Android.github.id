## 제네릭 (Generic)

### 제네릭의 필요성

- 출력시 엽력된 객체 타입으로 캐스팅 필요 -> 잘못된 객체 타입 캐스팅 : RuntimeException 발생 -> 컴파일 오류발생 안함.

```java
class Apple{}
class Pencil{}

class Goods {
    private Object object = new Object();

    public Object get() {
        return object;
    }

    public void set(Object object) {
        this.object = object;
    }
}

class Main {
    public static void main(String[] args) {
        Goods goods1 = new Goods();
        goods1.set(new Apple());    // Apple 저장
        Apple apple = (Apple)goods1.get();  // Object -> Apple

        Goods goods2 = new Goods();
        goods2.set(new Pencil());   //Pencil 저장
        Pencil pencil = (Pencil) goods2.get();  //Object -> Pencil

        Goods goods3 = new Goods();
        goods3.set(new Apple());
        Pencil pen = (Pencil) goods3.get(); // classCastException
    }
}
```



### Generic 클래스/인터페이스 <span style="color:#ff5a54">정의</span> 문법 구조

```java
접근지정자 class 클래스명<T> { }
접근지정자 class 클래스명<K, V> {}
```

T, K, V : 제네릭 타입변수

- 제네릭 타입 변수명
- 다수개 타입변수 사용가능
- 일반적으로 영대문자 하나를 사용

#### 대표적인 제네릭 타입변수

T : 타입(Type)
K : 키(Key)
V : 값(Value)
N : 숫자(Number)
E : 원소(Element)

```java
class MyClass<T> {
    private T t;
    public T get() {
        return t;
    }

    public void set(T t) {
        this.t = t;
    }
}

interface MyInterface<K,V> {
    abstract void setKey(K k);
    abstract void setValue(V v);
    abstract K getKey();
    abstract V getValue();
}
```

### Generic 클래스/인터페이스 <span style="color:#ff5a54">객체생성</span> 문법 구조

객체생성시 제네릭타입을 지정하지 않으면 올 수 있는 Type 중 최상위 클래스(Object)로 인식

```java
MyClass mc = new MyClass();
MyClass<Object> mc = new MyClass<>()();
```

### Generic 정의 및 객체 생성 및 사용

```java
class KeyValue<K, V> {
    private K key;
    private V value;

    public K getKey() {
        return key;
    }

    public void setKey(K key) {
        this.key = key;
    }

    public V getValue() {
        return value;
    }

    public void setValue(V value) {
        this.value = value;
    }
}

class Main {
    public static void main(String[] args) {
        KeyValue<String, Integer> kv1 = new KeyValue<>();
        kv1.setKey("사과");
        kv1.setValue(1000);
        String key1 = kv1.getKey();     // 사과
        int value1 = kv1.getValue();    // 1000
        
        KeyValue<Integer, String> kv2 = new KeyValue<>();
        kv2.setKey(404);
        kv2.setValue("Not Found(요청한페이지를 찾을 수 없음)");
        int key2 = kv2.getKey();        // 404
        String value2 = kv2.getValue(); // Not Found(요청한페이지를 찾을 수 없음)
        
        KeyValue<String, Void> kv3 = new KeyValue<>();
        kv3.setKey("키값만 사용");
        String key3 = kv3.getKey();     //키값만 사용
    }
}
```

제네릭의 기본 개념은 클래스 내에 사용되는 타입을 클래스의 정의 때가 아닌 객체 생성 때 정의하겠다는 의미이다.

### Generic Method

#### 제네릭 메서드 : <span style="color:#ff5a54">리턴타입</span> 또는 <span style="color:#ff5a54">매개 변수의 타입</span>을 제네릭 타입으로 선언

- 제네릭 타입 변수명 한개인 경우

```java
접근지정자 <T> T 메서드이름(T t) {}
```

- 제네릭 타입 변수명 두개인 경우

```java
접근지정자 <T, V> T 메서드이름(T t, V v) {}
```

- 매개변수에만 제네릭이 사용된 경우

```java
접근지정자 <T> void 메서드이름(T t) {}
```

- 리턴타입에만 제네릭이 사용된 경우

```java
접근지정자 <T> T 메서드이름(int a) {}
```



#### 제네릭 메서드의 <span style="color:#ff5a54">호출</span> 문법 구조

```java
class GenericMethods {
    public <T> T method1(T t) {
        return t;
    }
    public <T> boolean method2(T t1, T t2) {
        return t1.equals(t2);
    }
    public <K, V> void method3(K k, V v) {
        
    }
}

class Main {
    public static void main(String[] args) {
        GenericMethods gm = new GenericMethods();
        
        String str1 = gm.method1("안녕");     // String으로 유추
        boolean bool1 = gm.method2(2.5, 2.5);   // true
        
        gm.method3("국어", 80);       // K->String, V->Integer
    }
}
```

### Generic 타입 번위 제한(Bound)

#### 제네릭(Generic) 타입 범위 <span style="color:#ff5a54">제한(Bound)의 필요성</span>

```java
class Goods<T> {
    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }
}
```

만일 T가 <span style="color:#ff5a54">과일 클래스 또는 그 하위 클래스만 올 수 있도로고 한정</span>하면 Goods는 <span style="color:#ff5a54">과일류만을 저장</span>하는 제네릭 클래스로 정의 가능

```java
public <T> T genericMethod(T t) {
    // Object의 메서드만 사용 가능.
}
```

어떤 타입이 올지 모르기 때문에 내부에서는 <span style="color:#ff5a54">Object 메서드만 사용 가능</span> (할 수 있는 일이 매우 한정적)

T가 Number 클래스 또는 하위클래스(Integer, Double 등)만 가능하다고 제한하면 이를 클래스가 공통으로 가지고 있는 <span style="color:#ff5a54">Number 클래스의 메서드 사용 가능</span>

#### 제네릭 타입 범위 제한 종류

- <span style="color:#ff5a54">제네릭 클래스</span>의 타입 제한

```java
class Goods<T extends Fruit> {
    private T t;

    public T get() {
        return t;
    }

    public void set(T t) {
        this.t = t;
    }
}

class Main {
    public static void main(String[] args) {
        Goods<Apple> goods = new Goods<>();
//        Goods<Pencil> goods = new Goods<Pencil>();
    }
}
```

클래스/인터페이스 상관없이 extends로 타입을 지정하여 제한

- <span style="color:#ff5a54">제네릭 메서드</span>의 타입 제한

```java
class A {
    public <T extends String> void method1(T t) {
        t.charAt(0);
    }
}

interface MyInterface{
    public abstract  void print();
}

class B {
    public <T extends MyInterface> void method1(T t) {
        t.print();
    }
}

class Main {
    public static void main(String[] args) {
        A a = new A();
        a.method1("안녕");    //안

        B b = new B();
        b.method1(new MyInterface() {
            @Override
            public void print() {
                System.out.println("print() 구현");
            }
        });
    }
}
```

- 일반적인 <span style="color:#ff5a54">매개변수</span>로서의 <span style="color:#ff5a54">제네릭 클래스</span> 타입제한
  (메서드의 <span style="color:#ff5a54">매개변수</span>로 제네릭 클래스 객체가 오는 경우의 타입제한)

```java
class A { }
class B extends A { }
class C extends B { }
class D extends C { }

class Goods<T> {
    private T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }
}

class Test {
    void method1(Goods<A> g) {}             // A만 가능
    void method2(Goods<?> g) {}             // A, B, C, D 가능
    void method3(Goods<? extends B> g) {}   // B, C, D 가능
    void method4(Goods<? super B> g) {}     // A, B만 가능
}
```

### Generic의 상속

#### <span style="color:#ff5a54">제네릭 클래스</span>의 상속

부모클래스가 제네릭인 경우 자식클래스 또한 제네릭클래스 (<span style="color:#ff5a54">부모의 제네릭변수</span>를 그대로 물려받음)
<span style="color:#5aff54">자식클래스의 제네릭 타입의 개수는 부모랑 같거나 더 많을수 있음</span>

```java
class Parent<T> {
    T t;

    public T getT() {
        return t;
    }

    public void setT(T t) {
        this.t = t;
    }
}

class Child1<T> extends Parent<T> {}

class Child2<T, V> extends Parent<T> {
    V v;

    public V getV() {
        return v;
    }

    public void setV(V v) {
        this.v = v;
    }
}

class Main {
    public static void main(String[] args) {
        // 부모제네릭 클래스
        Parent<String> p = new Parent<>();
        p.setT("부모제네릭클래스");     // 부모제네릭클래스
        
        // 자식1 제네릭 클래스
        Child1<String> c1 = new Child1<>();
        c1.setT("자식1 제네릭클래스");  // 자식1 제네릭클래스
        
        // 자식2 제네릭 클래스
        Child2<String, Integer> c2 = new Child2<>();
        c2.setT("자식2 제네릭클래스");  // 자식2 제네릭클래스
        c2.setV(100);               // 100
    }
}
```

<span style="color:#ff5a54">제네릭 메서드</span>를 가진 <span style="color:#ff5a54">일반클래스</span>의 상속

부모클래스(일반클래스)가 제네릭메서드를 가진경우 자식클래스(일반클래스) 또한 제네릭메서드를 그대로 물려받음

```java
class Parent{
    <T extends Number> void print(T t) {}
}

class Child extends Parent {}

class Main {
    public static void main(String[] args) {
        // 부모 클래스의 제네릭 메서드 사용
        Parent p = new Parent();
        p.print(10);    // 10
        
        // 자식 클래스의 제네릭 메서드 사용
        Child c = new Child();
        c.print(5.8);   // 5.8
    }
}
```