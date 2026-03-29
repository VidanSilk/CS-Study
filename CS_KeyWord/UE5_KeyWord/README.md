## 언리얼 엔진 5 이론 및 키워드 정리 

언리얼 엔진 키워드를 정리하는 곳입니다 

[Unreal Engine5 Docs](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/unreal-engine-5-7-documentation)

--------------------------------------------------------------------------------
### Basic 

1. 좌표계
  - 유니티, DX, UE5등 왼손 좌표계 사용, 다만 언리얼 엔진은 **Z축이 위** <br>
  -  x축 Roll, y축 Pitch, z축 Yaw

UE5 좌표계 및 다양한 그래픽스 좌표계

<img src="https://github.com/VidanSilk/CS-Study/blob/main/CS_KeyWord/_img/3DCoordinateSystems.png"></img>

2. BluePrint / C++
  - **BluePrint**
      + 블루프린트는 에디터에서 노드 기반 인터페이스를 통해 게임 플레이 요소를 만들어주는 개념
      + 스크립팅 - 행동을 보다 손쉽게 정의 가능  
      + 빠른 반복 작업- 블루프린트 클래스를 더 빠르게 생성, 수정, 컴파일 및 테스트가 가능해서 프로토타이핑에 적합
      + 폭 넓은 접근성 - DataFlow가 시가적으로 표현되어, 쉽게 이해 가능하고, 진입 장벽이 낮음.
      + 향상된 검색 기능 - 쉽게 API, 에셋 레퍼런스를 검색하고 포함 가능
      + 안전한 메모리 모델 - 크래시를 방지할 수 있는 안전한 메모리 모델을 갖춤 
      + 단점 : 추가 기능이나 새로운걸 만들 때는, 기능을 제공 하지 않은 경우가 있음. 

      + [BluePrint Docs](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/introduction-to-blueprints-visual-scripting-in-unreal-engine)
  
  - **C++**
      + 빠른 런타임 - 블루프린트 로직보다 훨씬 빠름, 쉽게 빌드 가능 
      + 명확한 디자인이 가능 - C++에서 변수나 함수를 노출하면, 세밀한 제어를 통해 원하는걸 정확히 노출할 수 있고, 특정 함수/변수를 보호하고 클래스의 공식 API를 만들 수 있음. 즉, 지나치게 크고 따라잡기 어려운 블루프린트를 만들지 않아도 됨.
      + 광범위한 엑세스 - UE의 로우 레벨 함수 기능에 접근 가능 
      + 폭넓은 확장성 - 외부 시스템 및 라이브러리를 생성하고 인터페이싱 가능, 대규모 블루프린트 그래프와 비교하여 보다 쉽게 수정할 수 있음 
      + 강력한 제어 - 시스템, 리소스 및 복잡한 알고리즘에 대한 로우 레벨 제어 가능 
      + 원활한 협업 - 텍스트로 저장되기 때문에, 프로젝트 간의 비교, 병합 및 공유가 쉬움 
      + 뛰어난 디버깅 - 블루프린트 디버거 보다 더 강력한 디버깅 툴이 존재 

C++ 클래스를 블루프린트로 확장시켜 새로운 게임플레이 클래스를 코드로 구성하면, 레벨 디자이너는 이것을 기반으로 블루프린트를 통해 작업 및 변경이 가능함. <br>
C++ 클래스와 블루프린트 시스템간의 상화작용에 영향을 끼치는 지정자가 여러가지가 있음. 

---------------------------------------------------------------------------------
### UHT & UBT

3. UObject
  - C++로 class를 작성하면 UClass가 붙는데, UHT(Unreal Header Tool) 코드를 분석 <br>
  - 엔진 실행시, 분석한걸 토대로 리플랙션 데이터를 통해 CDO(Class Default Object)를 UObject로 인스턴스(객체)를 만들어주고 CDO복사해서 동적으로 사용하는 시스템.
  - 주기적으로 리플랙션에서 사용하지 않는 Object들은 GC(Garbage Collection)이 메모리 정리 해줌.
  
4. UHT / UBT
   
빌드를 시작하면 UBT가 실행되고, UBT은 C++ 컴파일러 실행전에 UHT를 실행하는데, UHT은 헤더 파일을 Parsing하고 UGameplayAbility_AutoAttack.generated.h,UGameplayAbility_AutoAttack.gen.cpp에 UPROPERTY, UFUNCTION, UCLASS매크로 선언된 정보들을 저장하는 역할을 수행 <br>
이 과정이 끝나면 UHT의 작업은 끝나게 되고, 일반 C++컴파일러가 UHT이 생성한 코드를 포함해서 C++컴파일을 수행 

   - **UHT(Unreal Header Tool)**
     + C++ 코들을 컴파일 하기전에 모든 헤더파일들을 순회하면서, 언리얼 리플렉션 시스템에 필요한 정보을 읽어들 후, .generate.h 파일과 gen.cpp파일을 Intermediate폴더에 생성하는 소프트웨어
     + 자체적으로 리플렉션 시스템을 구현하기 위해 UCLASS(),UPROPERTY(),UFUNCTION() 등의 매크로를 사용하고, 이 매크로를 해석해서 C++ 컴파일러가 알아들을 수 있게 코드를 생성(.generate.h, .gen.cpp)해주는 작업. 
       
   - **UBT(Unreal Build Tool)**
     + Uproject, Target.cs, Build.cs를 읽어서 어떻게 빌드할지 정함
     + UBT가 UHT를 실행해서 모든 헤더파일을 읽어서 언리얼 매크로들(UClass, UFuntion 등....)을 전부 다 찾아서, 이 정보를 바탕으로 리플랙션 시스템을 사용할 수 있게 컴파일러가 인식할 수 있게 .generated.h 파일을 생성
     + UBT가 C++ 컴파일러를 호출해서 캄파일 및 UHT가 만든 .generated.h 코드를 합쳐 컴파일하고, 이 결과물이 Binaries폴더에 exe랑 dll로 나옴.
     + 바이너리가 실행되면 엔진 또는 게임이 메모리에 올라가서, 모든 생성된 언리얼 오브젝트들 클래스 별로 각 기본 CDO가 메모리에 등록된다.
     + 참고로 **.generated.h는 항상 가장 마지막 밑에 include**해야한다. 아니면 인식이 제대로 안되어, 에러가 발생한다.

--------------------------------------------------------------------------------
###  Uneral Server & NetWork

5. Uneral Server / Unreal NetWork

일단 클라이언트 프로그래머가 언리얼 네티워크를 안다고 서버 프로그래머가 되는 것이 아니다. 클라 입장에서 서버를 공부하는 것으로, 클라가 하는 일들 중에서 '서버로 ~ 값이 변경 되었다고' 알려주는 것이다. 언리얼 네트워크는 일단 TCP로 구현 되어있다. <br>
다만, 일반적인 TCP 방식이 아닌 Replicate라는 시스템을 도입하였다. 그전에, 기본 네트워크 부터 알아보는 것이 좋을 것 같아서, OSI 7계층과 TCP/IP 4계층 프로토콜에 대해 설명하겠다. 

<img src="https://github.com/VidanSilk/CS-Study/blob/main/CS_KeyWord/_img/Osi7LayerandTCPIPImg.jpg" width="400" height="500"></img>

  - OSI 7계층
    1. 물리(Physical) 계층 → 주로, 전기적, 기계적 혹은 기능적인 특성을 이용해 통신 케이블로 데이터를 전송하는 곳으로, 통신 단위는 bit이다. 단순히 전송하기 때문에, 데이터가 무엇인지? 혹은 오류가 있는지 신경 쓰지 않는다. 대표적인 장비로 케이블, 리피터, 허브등이 있다. 
    2. 데이터 링크(DataLink) 계층 → 물리계층을 통해 송순신되는 정보의 오류와 흐름을 관리하여, 안전한 정보의 전달을 수행하는 역할을 한다. 즉, 통신에서의 오류도 찾아주고 재전송을 한다. 이 계층에서는 MAC 주소를 가지고 통신을 하고, 단위는 Frame이다. 대표적인 장비로 브릿지, 스위치가 있다.  정리하자면 브릿지나 스위치 장비를 통해 MAC 주소를 가지고 물리계층에서 받은 정보를 전달한다. 
    3. 네트워크(NetWork) 계층 → 데이터를 목적지까지 빠르고 안전하게 전달하는 기능(라우팅)을 한다. 네트워크 계층에서는 경로를 선택하고, 주소를 정하고 경로에 따라 패킷을 전달해주며, 라우터가 대표적인 장비이다. 여러 개의 노드를 거칠 때마다 경로를 찾아주는 역할을 하는 계층이기에, 다양한 길이의 데이터를 네트워크들을 통해 전달되고, 그 과정에서 전송 계층이 요구하는 서비스 품질(QoS)을 제공하기 위한 기능적, 절차적 수단으로 제공된다. 또한, 라우팅, 흐름 제어, 세그멘테이션, 오류제어, 인터네트워킹을 수행하기 때문에, 이 계층에서 동작하는 스위치가 있다. 정리하자면, 주소부여(IP), 경로설정(Route)를 해준다. 
    4. 전송(Transport) 계층 → 통신을 활성하기 위한 계층, TCP을 주로 이용하며 포트를 열어서 응용 프로그램들이 전송을 할 수 있게 해준다. 데이터가 4계층에 도착했으면, 해당 데이터를 하나로 합쳐서 5계층로 전송한다. 
    5. 세션(Session) 계층 → 데이터가 통신을 하기위한 논리적인 연결 계층이다. 세션 설정, 유지, 종료, 전송 중단시 복구 등의 기능이 있으며, End to End의 응용프로세스가 통신을 관리하기 위한 방법을 제공해준다. 동시 송수신 방식(duplex), 반이중 방식(half-duplex), 전이중 방식(Full Duplex)의 통신과 함께, 체크 포인팅과 유휴, 종료, 다시 시작 과정 등을 수행하는 계층이다. 
    6. 표현(Presentation) 계층 → 데이터 표현이 상이한 응용 프로세스의 독립성을 제공하고, 암호화를 한다. 보통 MIME 인코딩이나 암호화등의 동작이 여기서 이루어진다. 즉, 표현 계층에서는 데이터가 Text, Img, GIF, Png..등 구분을 하는 곳이다. 
    7. 응용(Application) 계층 → 최종 목적지로 HTTP, FTP, SMTP, POP3, IMAP, Telnet 등과 같은 프로토콜이 있다. 통신 패킷들은 나열한 프로토콜에 의해, 모두 처리가 된다. 흔히 사용하는 브라우저, 메일은 사용자가 프로토콜을 쉽게 사용해주는 응용 프로그램이다. 모든 통신의 양 끝단은 HTTP와 같은 프로토콜이지 응용프로그램이 아니기 때문에, 또한 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다 즉, 네트워크 소프트웨어의 UI 부분 및 사용자의 I/O부분이라고 생각하면 된다. 


  - TCP/IP 4계층
    - OSI 7 계층은 **데이터 통신에 필요한 계층과 역할을 정의한 참조 모델**이고, TCP/IP 4 계층은 인터넷에서 사용하는 프로토콜로, 단순한된 모델이라고 생각하면된다. 네트워크 연결 계층(Network Access Layer), 인터넷 계층(Internet Layer), 전송 계층(Transport Layer), 응용 계층(Application Layer)로 구성되어 있어 서로간의 간섭을 최소하고 유지와 보수에 있어 편리하다는 이점이 있다. 
    1. 네트워크 연결 계층(Network Access Layer) → 물리적인 데이터의 전송을 담당하는 계층으로 인터넷 계층과 달리 같은 네트워크 안에서 데이터가 전송된다. 노드 간의 신뢰성 있는 데이터 전송을 담당하며, 논리적인 주소가 아닌 물리적인 주소인 MAC을 참조해 장비간 전송을 하고, 기본적인 에러 검출과 패킷의 Frame를 담당한다.
    2. 인터넷 계층(Internet Layer) → 네트워크 상에서 데이터의 전송을 담당하는 계층으로 서로 다른 네트워크 간의 통신을 가능하게 하는 역할을 하기에 연결성을 제공해준다. 단말을 구분하기 위해 논리적인 주소로 IP 주소를 할당하게 되고 이 IP 주소로 네트워크 상의 컴퓨터를 식별하여 주소를 지정할 수 있도록 해준다. 네트워크끼리 연결하고 데이터를 전송하는 기기인 **라우터** 라고 하며, 라우터에 의한 네트워크 간의 전송을 **라우팅**'이라고 한다. 라우터가 내부의 라우팅 테이블을 통해 경로 정보를 등록하여 데이터 전송을 위한 최적의 경로를 찾고, 이렇게 출발지와 목적지 간의 데이터 전송 과정을 가리켜 End-to-End 통신이라고 부른다.
    3. 전송 계층(Transport Layer) → 통신 노드 간의 데이터 전송 및 흐름에 있어 신뢰성을 보장한다. 데이터를 적절한 어플리케이션에 제대로 전달되도록 배분하기에 End-to-End의 신뢰성을 확보한다. 전송 계층에 사용되는 대표적인 프로토콜로는 TCP와 UDP가 있다. TCP는 연결 지향형 프로토콜로 패킷에 하나의 오류라도 있으면 재전송을 위해 에러를 복구하는 반면, UDP는 패킷을 중간에 잃거나 오류가 발생해도 이에 대처하지 않고 계속해서 데이터를 전송하는 TCP에 비해 간단한 구조를 가지는 프로토콜이란 차이가 있다.
    4. 응용 계층(Application Layer) → 사용자와 가장 가까운 계층으로 사용자-소프트웨어 간 소통을 담당하는 계층이다. 웹 프로그래밍에서 보게되는 여러 서버나 클라이언트 관련 응용 프로그램들이 동작하는 계층이다. 주로 응용 프로그램들끼리 데이터를 교환하기 위한 계층이다. 
    
  - TCP/IP (Transmission Control Protocol & Internet Protocol)
    + **전송을 제어하는 프로토콜**로 전송 계층에서 사용하는 프로토콜, 장치들 사이에 논리적인 접속을 성립하기 위해 연결을 설정하여 **신뢰성**을 보장하는 연결형 서비스로, 네트워크에 연결된 컴퓨터 간에 데이터를 안정적이고, 순차적이며, 오류없이 교환을 한다.
    + 특징
      - **연결형 서비스**
        + 가상회선 방식을 제공하며, 3-way handshaking을 통해 연결을 설정하고 4-way handshaking을 통해 연결을 해제한다.
      - **흐름 제어(Flow Control)**
        + 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지한다.
        + 송신하는 곳에서 감당 안되게 많은 데이터를 빠르게 보내 수신하는 곳에서 문제가 발생하는 것을 막고
        + 수신자가 Window Size 값을 통해 수신량을 정할 수 있다.
      - **혼잡 제어(Congestion Control)**
        + 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하며, 정보량이 과다하면 패킷을 줄여 혼잡 붕괴 현상이 일어나는 것을 막아준다.
      - **신뢰성이 높은 전송(Reliable Transmission)**
        + Dupack-based retranmission → 정상적인 상황에서는 ACK(Acknowledgement) 값이 연속적으로 전송되어야 하고, ACK 값이 중복으로 올 경우 패킷 이상을 감지하고 재전송을 요청한다.
        + Timeout-based retransmission → 일정시간동안 ACK 값이 수신을 못할 경우 재전송을 요청한다.
      - **전이중/점대점 방식**
        + 전이중(Full Duplex) → 전송이 양방향으로 동시에 일어날 수 있다.
        + 점대점(Point to Point) → 각 연결이 정확히 2개의 종단점을 가지고 있다.
        + 멀티캐스팅이나 브로드 캐스팅을 지원하지 않는다. 

  - UDP (User Datagram Protocol)
    + **사용자 데이터그램 프로토콜**, 데이터그램은 독립적인 관계를 지니는 패킷을 뜻함.
    + UDP는 TCP와 달리, 비연결형 프로토콜이라, 할당되는 논리적인 경로가 없어 각각의 패킷은 다른 경로로 전송되고, 독립적인 관계를 지내게 되는데 이를 서로 다른 경로로 독립적으로 처리한다.
    + 특징
      - 비연결형 서비스로 데이트그램 방식을 제공
      - 데이터를 교환할 때 신호절차를 거치지 않음
      - UDP Header의 CheckSum 필드를 통해 최소한의 오류만 검출한다.
      - 신뢰성이 낮지만 TCP보단 전송 속도가 빠르다.
      - 주로 스트리밍 서비스 같은 연속성이 중요한 서비스에 사용함. 

즉, 신뢰성이 요구되는 애플리케이션은 TCP, 간단한 데이터를 빠른 속도로 전송할 때는 UDP를 사용한다. 

  - Dedicated Server
    + 서버 따로, 클라리언트 따로 즉, Server-Client 구조로 서버만 하는 것이 따로 있고, 클라이언트는 클라이언트의 역할만 한다.
    + 게임이 네트워크 멀티플레이어 세션을 호스팅하는 서버로 실행되며, 원격 클라이언트의 접속을 허용하지만, 로컬 플레이어는 없다.
    + 따라서, 더 효율적인 운영을 위해 그래픽, 사운드, 입력 및 기타 플레이어 중심 기능을 버린다. 주로 높은 지속성이나 보안, 대규모 멀티플레이어가 필요한 게임에 자주 사용된다.
    + 참고로, 비용이 비싸고 환경 설정이 어렵고 모든 플레이어가 별도의 컴퓨터에서 자체적으로 네트워크에 접속해야 한다. 

  - Listen Server
    + 한대의 컴퓨터가 호스트로, 서버역할 + 클라이언트 역할을 동시에 하는 것이다.
    + 게임이 네트워크 멀티플레이어 세션을 호스팅하는 서버로 실행되고, 원격 클라이언트의 접속을 허용한다.
    + 서버에 바로 로컬 플레이어가 있으며, 주로 가벼운 협동 및 경쟁 멀티플레이어에 자주 사용한다.
    + 다만, 서버로 실행하면서, 그래픽과 사운드 같은 플레이어 관련 시스템을 지원해야 하기 때문에, 추가 부하가 발생한다. 
  
  - P2P
    + 간단하게 이야기 하자면, 각각의 컴퓨터가 서버이면서 동시에 클라이언트까지 동작하는 방식이다. 

[언리얼 네트워킹 개요](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/networking-overview-for-unreal-engine)

<img src="https://github.com/VidanSilk/CS-Study/blob/main/CS_KeyWord/_img/Ue5NetWorkImg.png"  width="400" height="500"></img>

일단, 리플리케이션 개념은 **서버/클라 통신**과 관련된 개념이다. 모든 데이터를 서버에 둘 수도 없고, 모든 데이터를 클라에만 둘 수도 없으며, 클라이언트에 필요는 하지만 서버에서만 관리해야하는 데이터가 있을수도 있고 등등 여러 문제가 존재하는데 이를 정리한 것이 리플리케이션이다. <br>
언리얼 네트워크 시스템상에서 각 요소(Actor) 들은 크게 3가지로 나뉜다. Server Only, Server/Client Multy, ClientOnly <br>
Server Only는 게임 관리자에 해당하는 **GameMode**가 포함되며 Server/Client Multy는 게임 상태을 담당하는 **GameState** (점수, 스탯, 이동정보 등등) 하지만 중요한건 Server/Client Side **양측에 모두 존재하는 액터들**이다. <br>
물론 여기서도 몇가지가 나뉘어 지는데, 

1. 서버가 무조건 우선권을 가지는 경우 → 관리 권한이 존재 
2. 모든 클라이언트가 소유하고 있어야 하는 경우 → 관리 권한은 없으나 소유권은 있음
3. 지금 화면을 보고 있는 클라이언트의 정보만 있어야 하는 경우 → 관리 권한도 없고, 소유권도 없음

이런식으로 3가지 타입으로 구별이 가능하고 이를, **Authority, Autonomous, Simulated**라고 표현한다. 

[액터 롤 및 리모트 롤 4.27](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/actor-role-and-remoterole?application_version=4.27)
    
  + 언리얼 서버는 **복제(Replicate)** 로 서버에 내용을 클라이언트들에게 복제해주는데, 개발 할 때 클라이언트가 정해야하는 것은 어떤 것은 복제를 하고, 어떤 것은 복제를 안 해야하는지를 정하는거다
  + 리플레케이트 방식에는 변수와 함수가 있다.
    - 변수 → 리플리케이트, 렙노티파이 리플리케이트 서버에서 값이 변경되면 바로 클라이언트로 동기화 된다. 렙 노티파이 서버에서 값이 변경되면 값이 변경을 호출하고 일어날 때 바인드된 OnRep 함수를 호출한다.
    - 함수 → RPC(Remote Procedure Call)라 부르고, 방식에는 Server, Client, Multicast 3가지가 있다. 중요한 건, Server를 제외한 다른 2가지 (Client, Multicast) 서버에서만 동작이 정상적으로 된다. 클라이언트 서버로 호출할 때 Server를 사용한다.

함수를 구현할 때 서버랑 클라이언트를 구분하는 방법은 다음과 같다. <br>
Authority : 서버 <br>
Remote : 클라이언트 

추가적으로 **Travel**에 대해 알아보자. <br> 
멀티플레이어에서의 이동(Travel) 그러니까 멀티플레이어에서의 이동 작동방식에 대한 것이다. <br>
언리얼엔진에서의 이동 방식은 크게 2가지로 나눌 수 있는데, **매끄러운 방식인 Seamless과 매끄럽지 않은 방식인 Non-Seamless 방식**으로 나눌 수 있다. 이 둘의 차이점은 Seamless 방식은 블록이 없는 작업이 반면, Non-Seamless는 블록이 있는 호출이라는 점이다. <br> 
Client가 Non-Seamless이동 실행시, 클라이언트는 서버에서 접속을 끊은 다음 같은 서버에 다시 접속하여, 새로 로드할 맵을 준비한다. <br>
하지만, 언리얼 멀티플레이어 게임은 가급적으로 Seamless을 사용할 것으로 권하는데, 일반적으로 플레이어가 느끼기에 부드럽고, 재접속 프로세스 도중에 발생할 수 있는 문제를 피할 수 있기 때문이다. <br>
Non-Seamless가 일어날 수 없는 상황은 셋 정도 밖에 없다. 
1. 맵을 처음 로드할 때
2. 서버에 클라이언트로 처음 접속할 때
3. 멀티플레이어 게임을 끝나고 새 게임을 시작할 때

Travel을 위해 쓰이는 함수는 크게 UEngine::Browse, UWorld::ServerTravel, APlayerController::ClientTravel의 3가지이며, 어떻게 사용할지 약간 헷갈릴 수 있으니 하나씩 알아보는 것이 좋다. 
  
  - UEngine::Browse
    + 새 맵 로드시 **Hard Reset** 같은 것이다.
    + **항상 Non-Seamless 이동이 된다.**
    + 목적 맵에 이동하기전에 **Server가 현재 Client 접속을 끊는다**
    + Client는 **현재 서버에 접속이 끊긴다.**
    + Dedicated Server는 **다른 서버로 이동할 수 없으며**, 맵은 **반드시 Local**이어야 한다. 즉, URL이 될 수없다. 
  
  - UWorld::ServerTravel
    + Server 전용
    + Server를 새 월드/레벨로 점프시킨다.
    + 접속된 모든 Client도 따라간다
    + 멀티플레이어 게임에서는 맵 사이 이동할 때 쓰는 방식으로, 서버에서 이 함수 호출을 담당한다.
    + 서버는 접속된 모든 Client Player에 대해 APlayerController::ClientTravel를 호출해준다.
      
  - APlayerController::ClientTravel
    + Client에서 호출되면, 새 서버로 이동
    + 서버에서 호출되면, 특정 클라이언트더러 새 맵으로 이동하라 알려준다. (현재 서버에 접속은 유지)

6. 매끄러운 이동(Seamless Travel) 활성화 

Seamless Travel을 활성화 시킬려면, 트랜지션 맵을 구성해야 되고, UGameMapsSettings::TransitionMap 프로퍼티를 통해 이루어진다. 기본적으로 프로퍼티는 비어 있는데, 게임에서 이 프로퍼티가 비어있으면 트랜지션 맵에 기본 맵이 생성된다. <br>
트랜지션의 맵의 존재 이유는, 항상 (맵이 저장된) 월드가 로드되어 있어서, 새 맵을 로드하기 전에 이전 맵을 해제시킬 수 없기 때문이다. 그리고 맵이 매우 클 수 있어, 이전 맵과 새 맵을 동시에 메모리에 두는것이 좋지 않아, 트랜지션 맵이 생긴 것이다. <br>  
현재 맵에서 트랜지션 맵으로 이동한 다음 거기서, 최종 맵으로 이동하면 된다. 트랜지션 맵은 매우 작아, 현재 맵을 최종 맵으로 대체시키는 와중에 그다지 큰 부하가 걸리지 않는다. <br>
트랜지션 맵 구성이 완료되고나서, AGameModeBase::bUseSeamlessTravel을 true로 설정하면 Seamless Travel이 작동할 것이다. 

  - Seamless Travel Flow
    + Seamless Travel 실행시의 일반적은 흐름은 다음과 같다.
    + 1. 트랜지션 레벨에 지속되는 엑터를 마킹
    + 2. 트래지션 레벨로 이동
    + 3. 최종 레벨로 지속되는 엑터를 마킹
    + 4. 최종 레벨로 이동
  
  - Persisting Actors across Seamless Travel
    + Seamless Travel을 사용시 현재 레벨에서 새 레벨로 (지속) 액터를 가지고 가는 것이 가능하다. 예를 들어, 인벤토리 아이템, 플레이어와 같은 특정 엑터에 좋다.
    + 기본적으로 자동 지속되는 엑터는 다음과 같다.
    + GameMode 액터 (서버만), AGameModeBase::GetSeamlessTravelActorList를 통해 추가된 엑터
    + 유효한 PlayerState가 있는 모든 Controller (서버만)
    + 모든 PlayerController (서버만)
    + 모든 로컬 PlyerController (서버/클라이언트), 로컬 PlyerController에서 호출된 APlayerController::GetSeamlessTravelActorList를 통해 추가된 엑터

p.s 트랜지션 맵이란?
 멀티플레이어 환경에서 맵(레벨)을 변경할 때, 기존 맵에서 목적지 맵으로 매끄럽게(Seamless) 이동하기 위해 거쳐가는 중간 로딩용 맵

[UE5 Travelling in Multiplayer](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/travelling-in-multiplayer-in-unreal-engine)

--------------------------------------------------------------------------------
### UE5 Artificial Intelligence

7. UE5 AI

게임에서 중요한 요소중 하나가 상호작용이라고 생각하는데, 게임속에서 몬스터 어그로를 끌거나, 어려운 패턴을 가진 보스를 잡거나 혹은 NPC가 선택을 해서 세계선이 바뀌거나 등... 게임속의 룰/오브젝트와 상호작용하여 목표를 달성하는게 게임이고, 액터가 마치 지능이 있는 것처럼 행동하도록 설계하는 기능을 언리얼 엔진의 AI라고 한다. 또한, 인공지능은 여러가지 역할을 맡는데 아군 역할을 할 수도, 적군 역할을 할 수 있다. 다만 실제로 플레이어가 하는것 처럼 **똑똑한** 인공지능이 되어야하지, 멍청하면 게임성이 떨어진다. 
  
  + 언리얼의 AI는 보통 3가지 오브젝트로 구성된다.
  + AI Controller → 게임속 AI 액터를 찾아서 움직이도록 조작
  + Behavior Tree → 상황에 따라 판단하도록 알고리즘, 혹은 행동을 지정해주는 역할.
  + Black Board → AI의 뇌 역할로, AI가 해야하는 일의 정보와 Key 값 등의 저장하는 공간
  + 참고로, AI는 Build.cs에서 **AIModule**키워드를 추가해줘야하고, 보통은 AIController는 C++클래스로, Behavior Tree와 Black Board는 에디터에서 생성하는 편이다. 
  + [언리얼 인공지능](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/artificial-intelligence-in-unreal-engine)

8. Unreal Behavior Tree

언리얼 AI를 하기전에, Behavior Tree을 대해 알아야하는데, 이는 AI가 어떤 행동을 하는가?를 설정하기위해서 Behavior Tree라는걸 설정해줘야한다.   
  
  + Behaivor Tree
    - 행동트리는 본질적으로 AI의 프로세서로서, 각종 결과를 내리고 그 결과를 바탕으로 다양한 분기를 실행할 수 있다.
    - 즉, AI의 제어 엔티티의 동작을 설계하고 구현하기 위해 사용되는 시각적 프로그래밍 도구이며, Behavior Tree는 작업, 조건 및 제어 흐름을 나타내는 노드의 계층구조로 구성된다.
    - 크게 4가지로 나눌 수 있는데 Composite, Task, Decorator, Services
    - Composite → 컴포짓 노드는 분기의 루트와 분기가 실행되는 방식의 기본 규칙을 정의. 즉, 컴포짓에 Decorator를 적용하여 분기 진입을 수정하거나 실행을 중도에 취소할 수 있으며, 자식이 실행 중일 때만 활성화 되는 Service도 어태치할 수 있다. 
    - Decorator → 조건문 해당 분기를 실행하지 종료할지....등 여러 조건문을 만들 수 있다. 그리하여 조건식이라고도 하며, Composite이나 Task 노드에 어태치되어 트리에 있는 분기 또는 단일 노드를 실행할 수 있는지 여부를 정의한다. 
    - Services → Composite이나 Task에 어태치되며, 분기가 실행 중인 동안, 정의된 빈도로 실행된다. 보통 블랙보드의 확인 및 업데이트에 사용된다. 
    - Task → AI가 행동할 노드. 즉, AI를 이동하거나 블랙보드 값을 조정하는 등 각종 '작업'을 수행하는 노드이며, 테스크에 Decorator나 Services를 어태치할 수 있다. 

 + Black Board
   - AI의 두뇌로 생각할 수 있는데, 행동 트리가 결정을 내리는 데, 사용하는 Key 값을 저장한다. 즉, AI가 의사결정에 필요한 정보를 주고받도록 도와주는 데이터 저장소이다.
   
  + [언리얼 행동트리](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/behavior-tree-in-unreal-engine---user-guide)
  + 추가로 주의해야할 점이 있는데, 언리얼4에서는 블랙보드랑 행동 트리를 직접 다 지정해줘야 했지만, 언리얼5는 **행동 트리**만 설정하면 되기에 버전을 꼭 확인하는 것이 좋다.  
      
9. Unreal Navigation System

  - Unreal Engine Navigation System은 인공지능 에이전트가 경로 찾기를 사용하여 Level을 이동할 수 있게해주는 기능
  - Level내 콜리전 지오메트리에서 메시를 생성하여 메시를 타일로 분할하고 그 타일이 폴리곤으로 분할되어, 에이젠트가 목적지로 이동할 때 사용하는 그래프를 형성 및 각 폴리곤에는 에이전트가 전체 비용이 **가장 낮은 최적의 경로**를 판정하는데 사용할 비용을 할당한다.
  - 다양한 컴포넌트와 세팅이 포함되고, 폴리곤에 할당되는 비용 등 내비게이션 메시의 생성 방식을 수정 가능함, 이는 에이전트가 레벨 내에서 이동하는 방식에 영향을 미치고 플랫폼이나 다리 등 연속되지 않는 내비게이션 메시 영역을 연결할 수도 있다.
  - 또한, 3가지 생성 모드(Generation Modes) 가 포함되는데, **스태틱(Static) , 다이내믹(Dynamic) , 다이내믹 모디파이어만(Dynamic Modifiers Only)** 이다. 생성 모드는 내비게이션 메시가 프로젝트에서 생성되는 방식을 제어하고 필요할 때 이용할 수 있는 다양한 옵션을 제공해준다. 상호 속도 장애물(Reciprocal Velocity Obstacles, RVO) 과 크라우드 우회 매니저(Detour Crowd Manager) 두 가지의 에이전트 **회피 메서드**를 제공해주고, 회피 메서드는 에이전트가 게임플레이 중에 다이내믹한 장애물과 다른 에이전트 주변으로 지나다닐 수 있게 해준다.

------------------------------------------------------------------------------------------------
### UE5 PCG(Procedural Content Generation)

10. PCG

PCG란 언리얼 엔진에서 자체 프로시저럴 콘텐츠와 툴을 제작할 수 있는 툴세트인데, 건물이나 생물군 생성과 같은 에셋 유틸리티부터 전체 월드에 이르기까지 복잡성에 관계 없이 빠른 반복작업 툴과 콘텐츠를 빌드할 수 있도록 지원을 해준다. 

  - Point → PCG 그래프에 의해 생성되는 3D 스페이스 내 위치로, 종종 메시를 스폰하는데 사용된다. 트랜스폼, 바운드, 색, 밀도, 경사도, 시드에 대한 정보가 포함되며, 포인트에 사용자 정의 어트리뷰트를 할당할 수 있다.
  - Point Density → 다양한 그래프 노드에서 사용되는 값으로, 디버그 뷰에서 각 포인트의 그레이디언트로 표시되고, 해당 위치에 포인타가 존재할 확률을 나타내준다. Density가 0이면 검정, 1이면 흰색이다.

참고로, PCG를 사용하기위해서는 에디터에서 프로시저럴 콘텐츠 제너레이션 프레임워크(Procedural Content Generation Framework) 플러그인을 활성화 해줘야한다. <br>
[Unreal Engine PCG](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/procedural-content-generation-overview#important-concepts-and-terms)

  + PCG Component
    - PCG Component를 통해 Level를 샘플링할 수 있는데, 이 컴포넌트는 프로시저럴 노드 그래프의 인스턴스를 저장하고 에디터와 런타임 모두에서 Procedural Content의 생성을 관리한다.
    - 액터에 컴포넌트로 추가되거나, Procedural Content를 설정하는데 좋은 PCG 볼륨의 일부로 사용된다.
    
  + World Partition
    - 그리드를 사용하여 런타임 시 월드을 다이나믹하게 로드되고, 언로드 될 수 있는 셀로 분리 해주는 기능이지, 멀리 있는 산과 나무, 절벽 등 멀리 떨어져있고, 상호작용이 없는 액터를 계속 보이게 해야하는 경우도 있다. 
    - PCG Asset를 월드 파티션의 데이터 레이어 혹은 HOLD 레이어에 할당하면 PCG그래프는 액터를 생성하여, 동일한 데이터 레이어, 동일한 HLOD 레이어를 할당해준다.
  
HLOD는 Hierarchical Level of Detail로, 커스텀 HLOD 레이어를 사용하여 대량의 스태틱 메시 액터를 구성하고, 단일 프록시 메시와 머티리얼을 생성한다. 이 기법을 사용하면 언로드된 월드 파티션 그리드 셀을 시각화하고, 프레임 드로 콜 수를 줄이고, 퍼포먼스를 향상 시킬 수 있다. 주로 대규모 오픈월드에서 사용할 때 유용하다.  

------------------------------------------------------------------------------------------------------------------
### UE5 World Partition

11. World Partition

기존에 큰 맵을 만들려면 프로그래머가 직접 맵을 서브 레벨로 나누고나서, 레벨 스트리밍 시스템을 통해 플레이어가 랜드스케이프를 이동할 때 로딩, 언로딩을 했어야 했는데, 이러한 경우에 다수의 사용자가 파일을 공유할 때 종종 문제가 생겼고, 전체 월드의 컨텍스트를 파악하기가 어려웠다. 이에 대한 해결책으로 World Partition이 등장하였다. <br>
World Partition은 대규모 월드를 관리하기에 완벽한 솔루션인 자동 데이터 관리 및 거리 기반 레벨 스트리밍 시스템으로, 그리드 셀로 나눠진 단일 퍼시스턴트 레벨에 월드를 저장함으로써 대규모 레벨을 서브 레벨로 나눠야 했던 기존 방식과 달리, 스트리밍 소스로부터 떨어진 거리에 따라 그리드 셀을 로드하거나 언로드하는 자동 스트리밍 시스템이다.

월드를 단일 퍼시스턴트 레벨 파일에 저장하고, 환경설정 가능한 런타임 그리드를 사용해 공간을 스트리밍 가능한 그리들 셀로 다시 나눔으로써 작동을한다. 셀은 Player 같은 스트리밍 소스의 조냊로 인해 로딩, 언로딩이 된다. 엔진은 특정 시간에 플레이어가 보고 상호작용하는 부분만 로드해준다.

[Unreal Engine World Patition](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/world-partition-in-unreal-engine) <br>
[Unreal Engine World Partition DataLayers](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/world-partition---data-layers-in-unreal-engine) <br>
[Unreal Engine Level Instancing](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/level-instancing-in-unreal-engine) <br>
[Unreal Engine World Partition HOLD](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/world-partition---hierarchical-level-of-detail-in-unreal-engine) <br>
[Unreal Engine Using PCG with World Patition](https://dev.epicgames.com/documentation/ko-kr/unreal-engine/using-pcg-with-world-partition-in-unreal-engine) 

-----------------------------------------------------------------------------------------------------------------------------
### UE5 Chaos Physics 
