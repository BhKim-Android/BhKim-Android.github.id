## 컬렉션 프레임워크

### Collection(컬렉션)

- <span style="color:#ff5a54">동일한 타입을</span> 묶어서 관리하는 자료구조
- 저장 공간의 크기(capacity)를 동적으로 관리

### 프레임워크

- 클래스와 인터페이스의 모임 (라이브러리)
- 클래스의 정의에 설계의 <span style="color:#ff5a54">원칙 또는 구조</span>가 존재

### 컬렉션 프레임워크

- 리스트, 스택, 큐, 트리 등의 자료구조에 정렬, 탐색 등의 <span style="color:#ff5a54">알고리즘을 구조화</span> 해 놓은 프레임워크

#### <span style="color:#5a54ff">배열의 특징</span>

- 동일한 타입만 묶어서 저장 가능
- 생성시 <span style="color:#ff5a54">크기를 지정</span>하여야 하면 추후 변경불가 (<span style="color:#ff5a54">컬렉션과의 차이점</span>)

### 배열 vs 리스트

배열 : 저장공간크기 <span style="color:#ff5a54">고정</span>

리스트 : 저장공간크기 <span style="color:#ff5a54">동적 변환</span>

### List<E> 인터페이스 <span style="color:#ff5a54">구현 클래스</span>

- ArrayList<E>
- Vector<E>
- linkedList<E>

객체생성 방법

- List<E> interface의 <span style="color:#ff5a54">구현 클래스 생성자</span>로 <span style="color:#ff5a54">동적</span>컬렉션 생성 -> (데이터의 추가 삭제 가능)

- ```java
  List<E> aList1 = new ArrayList<E>();
  List<E> aList2 = new Vector<E>();
  List<E> aList3 = new LinkedList<E>();
  
  ArrayList<E> aList1 = new ArrayList();
  Vector<E> aList2 = new Vector();
  LinkedList<E> aList3 = new LinkedList();
  ```

- Arrays.asList(T ...) 메서드를 이용하여 <span style="color:#ff5a54">정적</span>컬렉션 생성 -> (데이터의 추가 삭제 불가능)

- ```java
  List<Integer> aList1 = Arrays.asList(1,2,3,4);
  aList1.set(1,7); // [1,7,3,4]
  ```

#### List<E> 컬렉션의 특징

- 배열처럼 수집(Collect)한 원소(Element)를 인덱스(index)로 관리
- List<E> 인터페이스의 주요 메서드

add(E element) : 매개변수로 입력된 원소를 리스트 마지막에 추가
Add(int index, E element) : index 위치에 입력된 원소 추가
addAll(Collection<? Extends E) c) : 매개변수로 입력된 컬렉션 전체를 마제막에 추가
addAll(int index, Collection<? Extends E> c) : index 위치에 입력된 컬렉션 전체를 추가

Set(int index, E element) : index 위치의 원소값을 입력된 원소로 변경.

Remove(int index) : index 위치의 원소값 삭제
Remove(Object o) : 원소중 매개변수입력과 동일한 객체 삭제
Clear() : 전체 원소 삭제

Get(int index) : index 위치의 원소값을 꺼내어 리턴
Size() : 리스트 객체 내에 포함된 원소의 개수
isEmpty() : 리스트의 원소가 하나도 없는지 여부를 리턴

toArray() : 리스트를 Object 배열로 변환
toArray(T[] t) : 입력매개변수로 전달한 타입의 배열로 변환