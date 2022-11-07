## ArrayList와 Vector

### ArrayList<E>

- List<E> 인터페이스를 구현한 <span style="color:#ff5a54">구현 클래스</span>
- 배열처럼 수집(collect)한 원소(Element)를 인덱스(index)로 관리하며 <span style="color:#ff5a54">저장용량(capacity)</span>을 동적 관리



#### 데이터 추가

```java
List<Integer> aList1 = new ArrayList();
aList1.add(3);
aList1.add(4);
aList1.add(5);
aList1.toString();          // [3, 4, 5]

aList1.add(1, 6);     // 1번째 index의 값을 추가.
aList1.toString();          // [3, 6, 4, 5]

List<Integer> aList2 = new ArrayList<>();
aList2.add(1);
aList2.add(2);
aList2.addAll(aList1);
aList2.toString();          // [1, 2, 3, 6, 4, 5]

List<Integer> aList3 = new ArrayList<>();
aList3.add(1);
aList3.add(2);
aList3.addAll(1, aList3);
aList3.toString();          // [1, 1, 2, 2]
```

모든 컬렉션 객체(List, Set 등)는 자신의 데이터를 모두 출력하도록 <span style="color:#ff5a54">toString()</span>메서드를 오버라이딩 해놓았음



#### 데이터 변경

```java
aList3.set(1, 5);
aList3.set(3, 6);
aList3.set(4, 7);       // IndexOutOfBoundsException
aList3.toString();      // [1, 5, 2, 6]
```



#### 데이터 삭제

```java
aList3.remove(1);
aList3.toString();          // [1, 2, 6]

aList3.remove(new Integer(2));
aList3.toString();          // [1, 6]

aList3.clear();
aList3.toString();          // []
```

Remove(<span style="color:#ff5a54">index</span> or <span style="color:#ff5a54">element</span>) : remove는 인덱스 또는 원소를 지정하여 삭제 할 수 있다.
Clear() : 모든 원소를 삭제 할 수 있다.

#### 데이터 정보 추출

```java
aList3.isEmpty();   // true

aList3.add(1);
aList3.add(2);
aList3.add(3);
aList3.size();      // 3

aList3.get(1);      // 2
```

isEmpty() : arraylist에 요소가 없는지를 검사 (size가 0인경우 true / 1이상의 경우 false)
Size() : 요소의 갯수
Get(index) : index번째의 요소를 return

#### 리스트 -> 배열

```java
Object[] object = aList3.toArray();
Arrays.toString(object);    // [1, 2, 3]

Integer[] integer1 = aList3.toArray(new Integer[0]);
Arrays.toString(integer1);  // [1, 2, 3]

Integer[] integer2 = aList3.toArray(new Integer[5]);
Arrays.toString(integer2);  // [1, 2, 3, null, null]
```

New Integer[0] : list의 size() >= 배열의 length -> list의 size 크기를 가지는 배열 생성
New Integer[5] : list의 size() <   배열의 length -> 배열 length 크기를 가지는 배열 생성



### Vector<E>

#### ArrayList<E>와의 공통점

- 동일한 타입의 객체 수집(Collection)
- 메모리의 동적 할당
- 데이터의 추가, 변경, 삭제 등의 메서드 제공

#### <span style="color:#ff5a54">ArrayList<E>와의 차이점</span>

- 모든 메서드가 <span style="color:#ff5a54">동기화(synchronized) 메서드로 구현</span>되어 멀티쓰레드에 적합 (Thread Safe)

#### 데이터 추가

```java
List<Integer> vector1 = new Vector<>();

vector1.add(3);
vector1.add(4);
vector1.add(5);
vector1.toString();     // [3, 4, 5]

vector1.add(1, 6);
vector1.toString();     // [3, 6, 4, 5]

List<Integer> vector2 = new Vector<>();
vector2.add(1);
vector2.add(2);
vector2.addAll(vector1);
vector2.toString();     // [1, 2, 3, 6, 4, 5]

List<Integer> vector3 = new Vector<>();
vector3.add(1);
vector3.add(2);
vector3.addAll(1, vector3);
vector3.toString();     // [1, 1, 2, 2]
```

#### 데이터 변경

```java
vector3.set(1, 5);
vector3.set(3, 6);
vector3.toString();     // [1, 5, 2, 6]
```



#### 데이터 삭제

```java
vector3.remove(1);  // [1, 2, 6]
vector3.remove(new Integer(2)); // [1, 6]
vector3.clear();    // []
```



