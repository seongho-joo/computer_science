<h1>Operating System (운영체제)</h1>

<details>
<summary>Table of Contents</summary>

- [사용자 운영체제 인터페이스(User Operating-System Interface)](#사용자-운영체제-인터페이스user-operating-system-interface)
  - [명령 인터프리터(Command-Interpreter) 🔗](#명령-인터프리터command-interpreter-)
  - [그래픽 사용자 인터페이스(Graphic User Interface) 🔗](#그래픽-사용자-인터페이스graphic-user-interface-)
  - [정리](#정리)
- [시스템 호출(System Call)](#시스템-호출system-call)
- [프로세스 개념(Process Concept)](#프로세스-개념process-concept)
  - [프로세스 상태(Process State) 🔗](#프로세스-상태process-state-)
  - [프로세스 제어 블록(Process Control Block) 🔗](#프로세스-제어-블록process-control-block-)
  - [스레드(Treads) 🔗](#스레드treads-)

</details>

---

## 사용자 운영체제 인터페이스(User Operating-System Interface)
사용자가 OS와 접촉하는 방식에는 여러 방법이 있다. 여러 방법 중 두 가지 기본적인 방법에 대해 정리하려고 한다. 

### 명령 인터프리터(Command-Interpreter) 🔗
Windows와 UNIX 같은 운영체제는 명령 인터프리터를 작업이 시작되거나, 사용자가(대화형 시스템 상에서) 처음 로그온 할 때 수행되는 특수한 프로그램으로 취급한다. 
선택할 수 있는 여러 명령어 해석기를 제공하는 시스템에서 여러 명령 인터프리터를 제공하는 시스템에서 이 해석기는 **셸(Shell)** 이라고 불린다.
예를 들면 UNIX나 Linux 시스템에서는 *Bourne Shell*, *C Shell* 등을 포함하여 사용자가 선택할 수 있는 여러 셸이 제공된다.

다음 이미지는 OSX의 zsh명령 인터프리터다.

![OSX zsh](https://user-images.githubusercontent.com/45463495/155632574-6f9f45a0-1d74-472a-a189-1921abe5bd8b.png)


명령 인터프리터의 중요한 기능은 사용자가 지정한 명령을 가져와서 그것을 수행하는 것이다. 이 명령어들은 두 가지 일반적인 방식으로 구현될 수 있다.

- 명령 해석기 자체가 명령을 실행할 코드를 갖고 있는 경우
  - 예) 한 파일을 삭제하기 위한 명령은 명령 해석기기가 자신의 코드의 한 부분으로 분기하고, 그 코드 부분이 매개변수를 설정하고 적절한 시스템 호추을 함
  - 각 명령이 자신의 구현 코드를 요구하기 때문에 제공될 수 있는 명령의 수가 명령 해석기의 크기를 결정
- 시스템 프로그램에 의해 대부분의 명령을 구현하는 경우
  - 명령 해석기는 전혀 그 명령을 알지 못함
  - 단지 메모리에 적재되어 실행될 파일을 식별하기 위해 명령을 사용
  - 예) 파일을 삭제하는 UNIX 명령 `rm`이라 불리는 파일을 찾아서, 그 파일을 메모리에 적재하고, 그것을 매개변수 `file name`으로 수행

### 그래픽 사용자 인터페이스(Graphic User Interface) 🔗
CLI를 통하여 사용자가 직접 명령어를 입력하는 것이 아니라 데스크톱이라고 특징지어지는 마우스를 기반으로 하는 윈도우 메뉴 시스템을 사용한다. 스마트나 휴대형 태플릿 컴퓨터는 보통 터치스크린 인터페이스를 사용한다.

### 정리
CLI 또는 GUI를 사용할 것인지는 개인의 선호에 따라 달려있다. 몇몇 GUI를 통해서는 시스템 기능의 일부만을 이용할 수 있고 자주 쓰이지 않는 나머지 기능은 CLI를 사용할 수 있는 사용자만이 이용할 수 있다. 게다가 CLI는 반복적으로 해야 하는 작업을 프로그래밍을 이용해 작업을 쉽게할 수 있다.

통상 실제 시스템 구조에선 제외되었다. 따라서 유용하고 친밀한 사용자 인터페이스를 설계하는 것이 운영체제의 직접적인 기능은 아니다.

## 시스템 호출(System Call)
**시스템 호출**은 운영체제에 의해 사용 가능하게된 서비스에 대한 인터페이스를 제공한다. 특정 저수준 작업은 어셈블리 명령을 사용하여 작성되어야 하더라곧 이러한 호출은 일반적으로 C와 C++ 언어로 작성된 루틴 형태로 제공된다.

**시스템 호출 과정**
<img width="1126" alt="image" src="https://user-images.githubusercontent.com/45463495/155646015-5f526f69-4624-4b58-8dc6-8b5158256df5.png">

위 사진처럼 파일을 복사하는 간단한 프로그램이라도 운영체제의 기능을 아주 많이 사용하게 된다. 종종 초당 수천 개의 시스템 호출을 수행하게 된다.

**시스템 호출의 유형**
시스템 호출은 **프로세스 제어**, **파일 조작**, **장치 조작**, **정보 유시 보수**와 **통신**과 **보호** 다섯 가지의 중요한 범주로 묶을 수 있다.

- 프로세스 제어(Process Control)
  - 끝내기(end), 중지(abort), 적재(load), 수행(execute)
  - 프로세스 생성 및 종료
  - 프로세스 속성 확인 및 설정
  - wait 이벤트, signal 이벤트
  - 메모리 할당 및 자유화
- 파일 관리(File Management)
  - 파일 생성 및 삭제
  - 열기, 닫기
  - 읽기, 쓰기, 위치 변경
  - 파일 속성 확인 및 설정
- 장치 관리(Device Management)
  - 장치 요구 및 방출
  - 읽기, 쓰기, 위치 변경
  - 장치 속성 확인 및 설정
  - 장치의 논리적 장착 또는 해제
- 정보 유지(Information Maintenance)
  - 시간과 날짜 설정과 확인
  - 시스템 데이터 설정과 확인
  - 프로세스, 파일, 장치 속성 확인 및 설정
- 통신(Communication)
  - 통신 연결 생성, 제거
  - 메시지 송신, 수신
  - 상태 정보 전달
  - 원격 장치 장착 및 해제
- 보호(Protection)
  - 권한 설정 및 확인
  - 유저의 접근 허용 및 접근 제한

**Windows와 Unix 시스템 호출의 예**
![image](https://user-images.githubusercontent.com/45463495/158323681-68208c27-e5f0-4675-810a-e09d55ce038a.png)

## 프로세스 개념(Process Concept)
디스크에 있는 것은 프로그램, 메모리에 적재된 것은 프로세스라 한다. 프로세스는 **Stack**, **Data**, **Heap**, **Text**로 나뉜다.

<img width="270" alt="image" src="https://user-images.githubusercontent.com/45463495/158524650-ee3f3b21-4db8-4d67-8226-33de80b34ff0.png">

- Text: 실행 코드
- Stack: 함수를 호출할 때 임시 데이터 저장소(ex: 함수 매개변수, 복귀 주소 및 지역 변수)
- Data: 전역 변수 저장
- Heap: 프로세스 실행 중 동적으로 할당되는 메모리

### 프로세스 상태(Process State) 🔗
<img width="950" alt="image" src="https://user-images.githubusercontent.com/45463495/158524563-e5ef796d-a9ca-47ae-bd65-40c8a3db4b29.png">

- new: 프로세스가 처음으로 생성됨
- ready: 프로세스가 프로세서에게 할당되기를 기다림
- running: 명령어들이 실행되고있음
- wating: 프로세스가 이벤트를 기다림
- terminated: 프로세스의 실행이 종료됨

### 프로세스 제어 블록(Process Control Block) 🔗
PCB는 각 프로세스마다 달라지는 모든 정보를 저장하는 저장소의 역할을 한다.

<img width="254" alt="image" src="https://user-images.githubusercontent.com/45463495/158524479-4c888a28-7b04-43ae-a1a8-f09e3d03f8a6.png">

- Process state: 프로세스의 상태
- Program counter: 프로세스가 다음에 실행해야 할 명령어의 주소를 가르킴
- CPU registers: 프로세스가 인터럽트 이후 올바르게 작업을 이어가기 위해 참조하는 CPU 레지스터 값
- CPU-scheduling information: 프로세스 우선순위, 스케줄 큐에 대한 포인터와 다른 스케줄 매개변수들을 포함한 정보
- Memory-management information: base 레지스터, limite 레지스터의 값, 페이지 테이블 또는 세그먼트 테이블 등과 같은 정보
- Accounting information: CPU 사용 시간, 시간 제한, 프로세스 번호 등의 정보
- I/O status information: 프로세스에게 할당된 입출력 장치 목록과 열린 파일의 목록 등의 정보

### 스레드(Treads) 🔗
싱글 스레드는 한번에 하나의 작업만 할 수 있다. 예를 들면 사용자가 워드 프로세스 프로그램을 실행한다면, 글자를 입력하면서 동시에 문법 검사기를 실행할 수 없다.