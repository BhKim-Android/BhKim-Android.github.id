---
layout: "default"
title: "Android 4대요소"
category: android
permalink: '/category/android'
---

# 첫글 연습 - Android 기본 요소.

### Android 4대 컴포넌트 개념정리

<p align="center">
  <img src="https://kairo96.gitbooks.io/android/content/pic2/2-1-1.jpg" />
</p>

- Activities : Activity는 유저 인터페이스를 포함한 하나의 화면을 나타냅니다.

- Services : 백그라운드에서 실행되는 컴포넌트로 장시간 실행되어야 하는 작업을 수행하거나 원격 프로세스를 위한 작업을 수행합니다.

- Broadcast receiver : 앱이 실행중이 아니더라도 시스템에서 발생한 이벤트를 앱에서 전달 받아야 할때 해당 컴포넌트를 등록하여 이벤트를 전달 받을수 있습니다.

- #### Content providers : 앱과 앱 저장소 사이에 데이터를 관리해주는 컴포넌트

각각의 컴포넌트에 자세한 내용은 다음 포스팅에 하나하나 정리하겠습니다.

#### Activity

액티비티는 UI를 제공하여 유저와 시스템의 상호작용을 돕는 컴포넌트입니다. Activity Class는 LifeCycle을 제공하며 액티비티 상태나 프로세스의 상태에 맞게 생명주기가 호출되며 거기에 필요한 동작을 정의할수 있습니다.

- **Intent**를 통하여 다른 Activity를 호출할수 있습니다. (타앱 포함)
- Activity는 하나의 Activity만 화면에 노출할수 있습니다. 호출된 Activity는 실행된 순서대로 Back stack에 순차적으로 저장되며 Activity 상태가 변하면 그에맞는 생명주기 메서드가 호출됩니다. 그리고 지금 노출중인 Foreground Activity를 종료하면 백 스텍에서 이전의 Activity가 팝됩니다.


<p align="center">
  <img src="https://developer.android.com/images/fundamentals/diagram_backstack.png?hl=ko" />
</p>




#### Service

서비스는 Background에서 작업을 처리할때 사용하는 컴포넌트입니다. UI를 제공하지 않지만 서비스 또한 메인 스레드에서 동작하기 떄문에 필요하다면 별도의 스레드를 추가하여 메인스레드에서 할수없는 작업들을 진행해야 합니다.

- 통신이 가능한 스레드를 추가하여 해당 스레드 안에서 Network 통신이 가능합니다.
- 서비스는 Background에서 실행되며 앱이 종료되어도 계속 동작합니다.
- 타 앱이 실행되어도 서비스는 계속 동작시킬수 있으며 타앱에서 서비스를 시작할수도 있습니다.
- 다른 컴포넌트를 서비스에 바인딩하여 상호작용할 수 있고 **프로세스간 통신(IPC)도 수행할수 있습니다.** 



#### Broadcast receiver

Android OS(시스템)에서 발생한 다양한 이벤트를 Broadcast receiver를 통해 필요한 데이터나 이벤트 발생을 전달합니다. 상태 표시줄에 알림(Notification)을 생성하여 유저에게 이벤트 내용을 전달합니다. 특정 시점에 앱의 활성화가 필요한 경우 브로드캐스트 리시버에 알람을 전달할경우 해당 이벤트가 발생하기전까지 앱을 실행할 필요가 없습니다. 

-  시스템 or 앱에서 설정한 알람이 발생할경우 해당 이벤트의 내용을 받아 처리할수 있습니다.



#### Content Provider

컨텐트 제공자는 데이터를 관리하고 타 어플리케이션의 데이터를 전달하는데 사용되는 컴포넌트입니다. 어플리케이션과 어플리케이션 사이의 DB공유를 위해 표준화된 인터페이스를 제공합니다.

- 시스템(ex.연락처) 또는 앱내의 DB를 적절한 권한을 부여받아 content provider를 통하여 읽고 쓸수 있습니다.
- 앱내에서 접근이 가능한 DB와 불가능한 DB를 설정할수있게 도와줍니다.
- URI를 할당하더라도 앱을 계속 실행할 피요가 없으므로 URI를 소유한 앱이 종료된 후에도 URI를 유지할 수 있습니다. 시스템은 URI를 소유한 앱이 해당 URI에서 앱의 데이터를 검색할 때만 실행되도록 하면 됩니다.





### Intent (비동기식 메시지)

Activity, Service, Broadcast receiver를 활성화 하거나 각각의 컴포넌트간 통신을 할때 Intent를 사용합니다. **또한 타앱의 컴포넌트와 통신할수도 있다.**

- Activity는 화면단위의 독립적인 컴포넌트이기 때문에 A(Activity)화면에서 B(Activity)화면으로 이동할때 Intent를 통하여 이동한다. **이때 필요한 Data나 Action을 전달(통신)할수 있고 Intent객체에 정의하여 전달한다.** 반대로 B화면이 종료될때 다시 A에 전달해야할 정보를 정의할수 있다.
- Service를 시작할때 Intent에 전달한 데이터와 실행할 서비스를 정의하여 startService()를 통하여 전달할수 있습니다.(5.0 API 21)이상에서는 JobScheduler로 서비스를 시작할 수 있습니다.)
- 브로드 캐스트는 sendBroadcast() 또는 sendOrdereBroadcast()에 전달하면 다른 앱에 브로드캐스트를 전달할 수 있습니다.



// Content provider의 쿼리를 수행하기 위해서는 ContentResolver에서 query()를 호출하면 됩니다.





### Manifests

안드로이드의 기본 구성 요소(4대 컴포넌트) 시작하려면 매니페스트에 해당 컴포넌트가 정의되어 있어야한다. 그외에도 사용자 권한(ex 인터넷 액세스, 스토리지 읽고 쓰기 액세스 등)을 정의해야 합니다.

- 실행할 컴포넌트를 선언하고 해당 컴포넌트의 특성을 정의해야 합니다.
  (Broadcast receiver의 경우 코드로 동적으로 등록할수 있습니다.)
- 앱에서 필요한 각종 권한을 선언해야 합니다.
- 앱이 사용될 최소 API레벨(Android OS버전별 API Level)을 지정해야 합니다.
- 앱에서 사용하는 하드웨어 및 소프트웨어 기능(ex 카메라, 블루투스, 멀티터치 등)을 선언해야 합니다.
- 앱이 링크되어야 하는 API 라이브러리(ex Google Maps라이브러리)를 선언해야 합니다. 





이외에 구성요소로는 리소스가 있다.











