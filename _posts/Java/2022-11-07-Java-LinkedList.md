## LinkedList

### ArrayList<E>와의 공통점

- 동일한 타입의 객체 수집(collection)
- 메모리의 동적할당
- 데이터의 추가, 변경, 삭제 등의 메서드

### ArrayList<E>와의 다른점

- 디폴트 저장공간(10)만 사용하여 <span style="color:#ff5a54">생성자로 저장공간의 크기 지정 불가</span>
- 데이터의 내부 저장방식이 index가 아닌 <span style="color:#ff5a54">앞뒤 객체의 위치 정보를 저장</span>

```java
List<E> aLinkedList1 = new LinkedList<Integer>();   // O
List<E> aLinkedList2 = new LinkedList<Integer>(20); // X
```



### 데이터 추가

```java
List<Integer> linkedList1 = new LinkedList<>();

linkedList1.add(3);
linkedList1.add(4);
linkedList1.add(5);     // [3, 4, 5]

linkedList1.add(1, 6);  // [3, 6, 4, 5]

List<Integer> linkedList2 = new LinkedList<>();
linkedList2.add(1);
linkedList2.add(2);
linkedList2.addAll(linkedList1);    // [1, 2, 3, 6, 4, 5]

List<Integer> linkedList3 = new LinkedList<>();
linkedList3.add(1);
linkedList3.add(2);
linkedList3.addAll(1, linkedList3); // [1, 1, 2, 2]
```



### 데이터 변경

```java
linkedList3.set(1, 5);
linkedList3.set(3, 6);  // [1, 5, 2, 6]
```



### 데이터 삭제

```java
linkedList3.remove(1);              // [1, 2, 6]
linkedList3.remove(new Integer(2)); // [1, 6]
linkedList3.clear();                // []
```



### 데이터 정보 추출

```java
linkedList3.isEmpty();   // true

linkedList3.add(1);
linkedList3.add(2);
linkedList3.add(3);     // [1, 2, 3]
linkedList3.size();     // size : 3
```



### 리스트 -> 배열

```java
Object[] object = linkedList3.toArray();
Arrays.toString(object);    // [1, 2, 3]

Integer[] integer = linkedList3.toArray(new Integer[0]);    // [1, 2, 3]
Integer[] integer = linkedList3.toArray(new Integer[5]);    // [1, 2, 3, null, null]
```





### 속도 비교

- 추가, 삭제(add, remove) : ArrayList<E> <span style="color:#ff5a54">늦음</span> / LinkedList<E> <span style="color:#ff5a54">빠름</span>
- 검색 (get) : ArrayList<E> <span style="color:#ff5a54">빠름</span> / LinkedList<E> <span style="color:#ff5a54">늦음</span>



### List<E>의 구현객체 정리

#### ArrayList<E>

- 저장 용량 자동 추가
- Index로 요소 관리

#### Vector<E>

- ArrayList와 동일한 특징
- 내부 메서드 동기화 적용 (<span style="color:#ff5a54">멀티쓰레드</span> 안정성)

#### LinkedList<E>

- 앞뒤 요소(Element)로 정보로 데이터 위치 관리
- <span style="color:#ff5a54">추가 삭제 속도 빠름</span>
- Index 정보가 없어 <span style="color:#ff5a54">검색 느림</span>