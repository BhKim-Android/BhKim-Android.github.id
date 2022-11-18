## LinkedHashSet<E>

- Set<E> 인터페이스를 구현한 구현 클래스 (<span style="color:#FF5A54">HashSet<E>의 자식 클래스, HashSet의 모든 기능 사용가능</span>)
- 수집(collect)한 원소(Element)를 집합의 형태로 관리하며 <span style="color:#ff5a54">저장공간(capacity)</span>을 동적관리
- <span style="color:#ff5a54">입력 순서와 출력의 순서는 동일</span> (단, 중복원소의 경우 추가되지 않음)



#### 데이터 추가

```java
Set<String> linkedSet1 = new LinkedHashSet<>();
linkedSet1.add("A");
linkedSet1.add("B");
linkedSet1.add("A");    // [A, B]

Set<String> linkedSet2 = new LinkedHashSet<>();
linkedSet2.add("B");
linkedSet2.add("C");
linkedSet2.addAll(linkedSet1);  // [B, C, A]
```

입력 순서대로 쌓임.



#### 데이터 삭제

```java
linkedSet2.remove("B"); // [C, A]
linkedSet2.clear(); // []
```



#### 데이터 정보 추출

```java
linkedSet2.isEmpty();   // true

Set<String> linkedSet3 = new LinkedHashSet<>();
linkedSet3.add("A");
linkedSet3.add("C");
linkedSet3.add("B");

linkedSet3.contains("B");   // true
linkedSet3.contains("D");   // false

linkedSet3.size();  // 3

Iterator<String> iterator = linkedSet3.iterator();
while (iterator.hasNext()) {
    iterator.next();    // A B C
}
```



#### Set to Array

```java
Object[] objArray = linkedSet3.toArray();   // [A, C, B]
String[] strArray1 = linkedSet3.toArray(new String[0]); // [A, C, B]
String[] strArray2 = linkedSet3.toArray(new String[5]); // [A, C, B, null, null]
```





### TreeSet<E>

- Set<E> 인터페이스를 구현한 구현 클래스
- 수집(collect)한 원소(Element)를 집합의 형태로 관리하며 <span style="color:#ff5a54">저장공간(capacity)</span>을 동적관리
- <span style="color:#ff5a54">입력 순서와 관계없이 크기순으로 출력</span> (저장원소(Element)는 대소비교가 가능해야 함)



#### Set<E>으로 객체타입을 선언하는 경우 <span style="color:#ff5a54">추가된 정렬/검색 기능사용 불가</span> 추가된 정렬기능을 사용하기 위해선 <span style="color:#ff5a54">TreeSet<E> 객체 타입 선언</span>

```java
Set<String> treeSet = new TreeSet<>();  // Set<E> 메서드만 사용가능.
TreeSet<String> treeSet1 = new TreeSet<>(); // Set<E> 메서드 + 추가된 정렬/검색 기능 메서드
```



#### 데이터 검색

```java
TreeSet<Integer> treeSet = new TreeSet<>();
for (int i = 50; i > 0; i -= 2) {
    treeSet.add(i); // [2, 4, 6, ..., 50]
}
treeSet.first();    // 2
treeSet.last();     // 50
treeSet.lower(26);    // 24 <- 매개변수로 입력된 원소보다 작은 가장 큰 수.
treeSet.higher(26);     // 28 <- 매개변수로 입력된 원소보다 큰 가장 작은 수.
treeSet.floor(25);      // 24 <- 매개변수로 입견된 원소보다 같거나 작은 가능 큰 수
treeSet.floor(26);      // 26 <- 매개변수로 입견된 원소보다 같거나 작은 가능 큰 수
treeSet.ceiling(25);    // 26 <- 매개변수로 입력된 원소보다 같거나 큰 가장 작은 수
treeSet.ceiling(26);    // 26 <- 매개변수로 입력된 원소보다 같거나 큰 가장 작은 수
```



#### 데이터 출력

```java
treeSet.size(); // 25
for (int i = 0; i < treeSet.size(); i++) {
    treeSet.pollFirst();    // 2 4 6 ... 50
}
treeSet.size(); // 0

for (int i = 50; i > 0; i-=2) {
    treeSet.add(i);
}
treeSet.size(); // 25
for (int i = 0; i < treeSet.size(); i++) {
    treeSet.pollLast(); // 50 48 46 ... 2
}
treeSet.size(); // 0
```

- pollFirst : Set 원소들 중 가장 작은 원소값을 꺼내어 리턴
- pollLast : Set 원소들 중 가장 큰 원소값을 꺼내어 리턴



#### 데이터 부분집합(SubSet) 생성

```java
for(int i = 50; i > 0; i-=2) {
    treeSet.add(i); 
}
SortedSet<Integer> sSet = treeSet.headSet(20);  // [2, 4, 6, ..., 18] default는 toElement 미포함

NavigableSet<Integer> nSet = treeSet.headSet(20, false);    // [2, 4, 6, ..., 18] toElement 까지(inclusize : 미포함) 정렬
nSet = treeSet.headSet(20, true);   // [2, 4, 6, ..., 20] toElement 까지(inclusize : 포함) 정렬

sSet = treeSet.tailSet(20); // [20, 22, 24, ..., 50] 20부터 정렬

nSet = treeSet.tailSet(20, false);  // [22, 24, 26, ..., 50]
nSet = treeSet.tailSet(20, true);   // [20, 22, 24, ..., 50]

sSet = treeSet.subSet(10, 20);  // [10, 12, 14, 16, 18]
        
nSet = treeSet.subSet(10, true, 20, false); // [10, 12, 14, 16, 18] 
nSet = treeSet.subSet(10, false, 20, true); // [12, 14, 16, 18, 20]
```



#### 데이터 정렬

```java
TreeSet<Integer> treeSet = new TreeSet<>();
for (int i = 50; i > 0; i -= 2) {
    treeSet.add(i); // [2, 4, 6, ..., 50]
}

NavigableSet<Integer> descendingSet = treeSet.descendingSet();  // [50, 48, 46, ..., 2]
descendingSet = descendingSet.descendingSet();  // [2, 4, 6, ..., 50]
```

- descendingSet : 내림차순의 의미가 아니라 <span style="color:#ff5a54">현재 정렬 기준을 반대로 변환</span>

#### TreeSet<E>에서 크기비교

##### Integer

```java
TreeSet<Integer> treeSet1 = new TreeSet<>();
Integer intValue1 = new Integer(20);
Integer intValue2 = new Integer(10);    // intValue1 > intValue2
treeSet1.add(intValue1);
treeSet1.add(intValue2);    // [10, 20]
```



##### String

```java
TreeSet<String> treeSet2 = new TreeSet<>();
String str1 = "AB";
String str2 = "CD";     // str1 < str2
treeSet2.add(str1);
treeSet2.add(str2);
treeSet2.toString();    // [AB, CD]
```



##### TreeSet<E>에서 크기비교

```java
class MyClass{
    int data1;
    int data2;
    public MyClass(int data1, int data2) {
        this.data1 = data1;
        this.data2 = data2;
    }
}

void fun() {
    TreeSet<MyClass> treeSet = new TreeSet<>();
    MyClass myClass1 = new MyClass(2, 5);
    MyClass myClass2 = new MyClass(3, 3);
    treeSet.add(myClass1);  // 예외 발생
    treeSet.add(myClass2);  // 예외 발생
}
```

<span style="color:#ff5a54">예외발생</span> : 크다 / 작다의 기준을 제공해주어야 함.

#### 해결방법

- Comparable<T> interface 구현
- TreeSet 생성자 매개변수로 Comparator<T> 객체 제공



#### MyClass 객체에 크기비교기능 부여

```java
class MyComparableClass implements Comparable<MyComparableClass> {
    int data1;
    int data2;
    
    public MyComparableClass(int data1, int data2) {
        this.data1 = data1;
        this.data2 = data2;
    }

    @Override
    public int compareTo(MyComparableClass myComparableClass) {
        if (data1 < myComparableClass.data1) { return -1; }
        else if (data1 == myComparableClass.data1) { return 0; }
        else return 1;
    }
}
```

Int compareTo(T t)

-  매개변수 t 보다 <span style="color:#ff5a54">작은</span> 경우 : <span style="color:#ff5a54">-1</span>
-  매개변수 t와 <span style="color:#ff5a54">같은</span> 경우 : <span style="color:#ff5a54">0</span>
-  매개변수 t 보다 <span style="color:#ff5a54">큰</span> 경우 : <span style="color:#ff5a54">1</span>

compareTo(MyComparableClass myComparableClass) : data2는 상관없이 data1만으로 크기를 결정하는 예.

```java
TreeSet<MyComparableClass> treeSet = new TreeSet<>();
MyComparableClass myComparableClass1 = new MyComparableClass(2, 5);
MyComparableClass myComparableClass2 = new MyComparableClass(3, 3); 
// MyComparableClass1 < myComparableClass2

treeSet.add(myComparableClass1);
treeSet.add(myComparableClass2);

for (MyComparableClass mcc : treeSet) {
    mcc.data1;  // 2 3
}
```

```java
TreeSet<MyClass> treeSet = new TreeSet<>((myClass1, myClass2) -> {
    if (myClass1.data1 < myClass2.data1) return -1;
    else if (myClass1.data1 == myClass2.data1) return 0;
    else return 1;
});

MyClass myClass1 = new MyClass(2, 5);
MyClass myClass2 = new MyClass(3, 3);   // myClass1 < myClass2
treeSet.add(myClass1);
treeSet.add(myClass2);
for (MyClass myClass : treeSet) {
    myClass.data1;  // 2 3
}
```



### Set<E> 컬렉션 - TreeSet<E>

#### HashSet<E>

- hashCode(), equals()를 통해 중복 체크
- 입력순사 != 출력순서

#### LinkedHashSet<E>

- HashSet의 기본 기능 포함
- 입력순서 = 출력순서

#### TreeSet

- Set<E>의 기본 기능 + 정렬/검색 기능 추가
- 출력순서 (오름차순)

```java
Set<String> hashSet = new HashSet<>();
hashSet.add("다");
hashSet.add("마");
hashSet.add("나");
hashSet.add("가");   // [가, 다, 마, 나]

Set<String> linkedHashSet = new LinkedHashSet<>();
linkedHashSet.add("다");
linkedHashSet.add("마");
linkedHashSet.add("나");
linkedHashSet.add("가"); // [다, 마, 나, 가]

Set<String> treeSet = new TreeSet<>();
treeSet.add("다");
treeSet.add("마");
treeSet.add("나");
treeSet.add("가");   // [가, 나, 다, 마]
```