## 클래스 외부 구성요소

### 패키지(Package)

- 프로젝트의 <span style="color:#FF5A54">하위폴더</span>
- 클래스 파일을 목적별로 묶어서 관리
- 유사 기능을 갖는 Class를 각 패키지별로 구분 가능(비추천, 라이브러리 예외)

### 임포트(Import)

- 다른 패키지의 클래스를 참조할때 사용.
- package와 main class 사이에 위치.
- 특정 클래스를 임포트 할수도, 그 클래스가 포함된 패키지를 임포트 할수도 있음.
- 패키지가 다른 <span style="color:#FF5A54">동일한 이름의 클래스</span>는 두개 이상 import 불가
- 패키지의 <span style="color:#FF5A54">모든 클래스를 한번에 임포트</span> 하기 위해서는 *사용.
  (Ex. Import abc.bcd.*)

### 외부 클래스

- 클래스의 외부에서 정의.
- 동일한 파일에 작성된 클래스는 동일한 패키지내의 클래스로 간주.
- 하나의 파일에 작성된 외부클래스는 다른 패키지에서 사용불가.(adapter pattern diffutil)

