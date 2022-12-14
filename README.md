# GameProgramming-Pattern
"게임 프로그래밍 패턴 - 더 빠르고 깔끔한 게임 코드를 구현하는 13가지 디자인 패턴"  (로버트 나이스트롬 저자) 내용 정리


<br>

# 디자인 패턴

## 명령(Command) 패턴

- 명령 패턴은 메서드 호출을 실체화한 것임. 즉,함수의 호출을 객체로 감쌌다는 의미

- 메서드의 호출을 추상화하는 패턴으로 코드가 깔끔해질 수 있지만 난해해질 수 있음

- 명령 패턴을 사용하면 undo와 redo를 쉽게 구현할 수 있음. 가장 최근의 명령을 기억하며 명령 목록을 유지하고 "현재" 명령이 무엇인지만 알고 있으면 됨
유저가 명령을 실행하면 새로 생성된 명령을 목록 맨 뒤에 추가하고，이를 "현재" 명령으로 기억하도록 함

  - 실행취소(undo)를 선택하면 현재 명령을 실행 취소하고 명령을 가리키는 포인터를 뒤로 이동시키고, 재실행(redo)를 선택하면 포인터를 다음으로 이동시킨 후 해당 포인터를 실행함

  - 유저가 undo를 여러 번 한 뒤에 새로운 명령을 실행하려고 하면 현재 명령을 가리키는 포인터 뒤에 새로운 명령을 추가하고 그 뒤에 있는 명령들은 삭제시킴

![undo, redo](https://user-images.githubusercontent.com/71704350/202890639-52a88a1d-5fb3-4181-ab4f-773a63cec73d.JPG)

- Command 패턴을 함수형에서 사용하지 않고 주로 클래스에서 사용하는 이유

  - C++11에 도입된 람다는 메모리를 직접 관리해야 하기 때문에 사용하기가 까다로움. 누가 누굴 호출했는지 알기 쉽지 않음


## 경량(Flyweight) 패턴

- 대량 데이터를 찍어내기 위해 사용하는 패턴 (<u>**최적화**</u>와 관련)

- 메시, 텍스처 등 공통적인 부분을 가진 인스턴스들을 많이 렌더링 해야 할 때 공통적인 부분은 묶어서 메모리를 공유시키는 방법

  - 공유시키지 않고 각 객체를 하나하나 렌더링하면 한 프레임에 GPU로 전달해야 하는 양이 너무 많음
 
  - 공유 객체 하나를 만들어 각 인스턴스들이 그 객체를 참조하는 형태로 만들고 위치나 회전, 스케일 같은 변수는 따로 지정해 줌
  
![경량](https://user-images.githubusercontent.com/71704350/202898556-f6ebb89b-f1a3-4538-85f4-1f338239c3ec.JPG)

- 공통적인 부분을 묶어 메모리를 공유하는 것이 **Flyweight**

- Flyweight의 매개변수를 한번에 처리하는 것이 **인스턴싱**. 즉, 렌더링 파이프라인에 100번의 과정을 거쳐 그려야 할 인스턴스들을 묶어서 한 번에 보내 1번의 과정으로 모두 그리는 것

## 프로토타입(Prototype) 패턴

- 기존 인스턴스를 복제해 새로운 인스턴스를 만드는 방법으로 경량 패턴과 혼동하기 쉬움 (원본을 두고 복사하는 방법임)

![프로토타입](https://user-images.githubusercontent.com/71704350/203064763-a4e0d0df-3316-42e5-8106-7294d52717f2.JPG)

- Spawner 클래스 내부에 Monster 객체를 숨기고 이 객체를 이용해 자기와 같은 Monster 객체들을 복제해주는 역할을 하게 만듦

  - A가 몬스터가 필요하면 스포너를 통해 원형을 받아 복사해서 필요한 기능을 붙여 사용함
  
  - 여기서 공통되는 대량 데이터에 Flyweight 패턴을 적용할 수도 있음

## 관찰자(Observer) 패턴

- MVC(모델-뷰-컨트롤러)패턴의 기반이 관찰자 패턴

- 게임 개발에서는 업적 달성 시스템 같은 것을 관찰자 패턴으로 구현할 수 있음

  - 업적을 이벤트로 따로따로 관리한다면 관리하기가 너무 어려워짐
  
  - 몬스터 100마리를 죽이는 업적이라면, 몬스터에 카운팅 변수를 넣어야 함... 맵 개척 업적이라면, 맵을 이동할 때마다 그것을 관리하는 쪽에 넣어줘야 함... ==> 이것이 너무 많아지면 관리하기 어려워짐
  
  - 따라서 분산시키는 것이 아니라 통합해서 관리하는 것이 관찰자로 특정 기능을 담당하는 코드를 한 곳에서 관리하는 것
  
- 업적의 이벤트가 발생되면, 관찰자 클래스에서 알림(Notify)를 발생시키고 알림을 제어해 알림을 받은 클래스에서 내부 함수를 콜하게 만듦

![관찰자](https://user-images.githubusercontent.com/71704350/203552600-8ad50419-5d20-490f-a517-d8abae9ff6e7.JPG)

- 작은 문제점

  - 느리다는 편견(클래스만 많아지고 느려진다고 생각함)을 갖고 있지만 정적 호출보다 늦을 뿐 성능에 민감한 코드가 아니라면 문제가 될 정도는 아님
  
  - 동적할당이 많이 일어날 수 있지만 무시할 수 있는 정도임

## 싱글턴(Singleton) 패턴

<u> !주의사항으로 득보다 실이 많은 패턴임 </u>

- 오직 한 개의 클래스 인스턴스만 갖도록 보장하는 패턴

- 로깅, 콘텐츠 로딩, 게임 저장 등 내부 시스템에서 파일 시스템 래퍼 클래스를 사용할텐데, 이들 시스템에서 파일 시스템 클래스 인스턴스를 따로 생성할 수 없다면, 파일 시스템에 접근하는 방법은 싱글턴 패턴

  - 하나의 인스턴스만 생성하는 것에 더해 싱글턴은 그 인스턴스를 전역에서 접근할 수 있는 메서드를 제공함 --> 즉, 전역 접근점을 제공

  - 이를 통해, 누구든 어디서든 인스턴스에 접근할 수 있음

- 싱글턴을 왜 사용하는가?

  - 한 번도 사용하지 않는다면, 아예 인스턴스를 생성하지 않음

  - 런타임에 초기화

    - 싱글턴의 대안으로 정적 멤버 변수를 많이 사용하는데, 정적 멤버 변수는 main함수를 호출하기 전에 자동 초기화되는 문제가 있어 프로그램이 실행된 다음에야 할 수 있는 정보를 활용할 수 없음
    - 게으른 초기화는 이런 문제를 해결해 줌, 싱글턴은 최대한 늦게 초기화되기 때문에 클래스가 필요로 하는 정보가 준비되어 있음
   
  - 싱글턴은 상속할 수 있음

- 싱글턴의 문제점...

  - 전역 변수

    - 코드를 이해하기 어렵게 함
    
    - 커플링을 유발함
    
    - 멀티스레딩 같은 동시성 프로그래밍에 적합하지 않음★
    
  - 싱글턴은 문제가 하나뿐일 때도 두 가지 이상의 문제를 풀려고 함
  
    - 클래스가 커지며 전역 변수가 많아지고 커플링이 많아짐
    
  - 게으른 초기화는 제어할 수 없음, 어디에서 초기화될 지 모름 --> 메모리 제어가 힘듦★
 
- 따라서 싱글턴을 사용할 때는 신중하자!

  - 꼭 클래스를 이용해 구현해야 할까?
  
    - Manager클래스를 이용해 관리하는 상황 같을 때, 인스턴스를 만들지 않아도 되는 상황이라면 Manager클래스를 없애고 Origin클래스에 작동 코드를 옮겨 객체가 스스로 챙기게 만들자. 그것이 OOP
    
  - 초기화 시점은 언제 잡을 것인가?
    
  - 오직 한 개의 클래스 인스턴스만 갖도록 보장
  
  - 싱글턴을 대체할 패턴 --> 하위 클래스 샌드박스 패턴, 서비스 중개자 패턴을 이용
  
## 상태(State) 패턴

  - 객체 내부 상태에 따라 스스로 행동을 변경할 수 있게 허가하는 패턴 --> 객체는 마치 자신의 클래스를 바꾸는 것처럼 보임
  
  - 자기 스스로 상태를 판단해 행동하도록 하는 것이 상태 패턴임, 유한 상태 기계(FSM)와 밀접한 관련
  
  - 유한 상태 기계 (FSM): 동작이나 행위 같은 유한한 개수의 상태를 가질 수 있는 오토마타

    (오토마타: 해당 객체는 자신의 상태에 따라 다음 객체의 상태를 정의함)
  
    - 유한 상태는 이전의 상태를 알 수 없음. 이를 해결한 것이 푸시다운 오토마타
    
    - 푸시다운 오토마타는 스택으로 관리하므로 이전의 상태를 알 수 있음
  
    - FSM의 요점
    
      - 가질 수 있는 '상태'한정 ==> 서기, 점프, 엎드리기...
      
      - 한 번에 '한 가지'상태만 될 수 있음
      
      - '입력'이나 '이벤트'가 기계에 전달됨
      
      - 각 상태에는 입력에 따라 다음 상태로 바뀌는 '전이'(transition)가 있음
      
      - Enum과 Switch ~ case 문으로 구현
      
    - FSM을 사용하면 좋을 때
    
      - 내부 상태에 따라 객체 동작이 바뀔 때
      
      - 상태 선택지가 많지 않을 때
      
      - 객체 입력이나 이벤트에 따라 반응할 때
            
    - FSM의 문제점
    
      - 상태 조합이 안됨, 예를 들어 점프 중에 공격하기...
      
      - 상태가 많아지면 매우 복잡해짐 (거미줄처럼 여기저기 얽혀 있는 상태들)
  
    - FSM의 확장 버전이 많이 있지만, 요즘에는 비헤이비어 트리를 사용함
    
  - 상태 패턴은 클래스를 사용하기 때문에 포인터에 저장할 실제 인스턴스가 필요함

    - 정적 객체 하나만 만들면 됨 --> 여러 FSM이 동시에 동작하더라도 상태 기계는 모두 같으므로 인스턴스 하나를 같이 사용하면 됨

<br>

***

<br>

# Sequence pattern (순서 패턴)

## 이중 버퍼(Double buffering)

- 본질적으로 컴퓨터는 순차적으로 동작을 하는데, 사용자 입자에서는 순차적 혹은 동시에 진행되는 여러 작업을 한 번에 모아서 봐야 할 때가 있음

  - 예를 들어, 게임에서 렌더링을 할 때 유저에게 보여줄 게임 화면을 그리려고 하는데 멀리 있는 산, 구불구불한 언덕, 나무 전부를 한 번에 보여주는데 이 과정에서 중간 과정이 보이면 몰입할 수가 없음
  
  - 이를 부드럽고 빠르게 업데이트 되게 하는 것이 이중 버퍼
  
![이중버퍼](https://user-images.githubusercontent.com/71704350/204803094-399a8e37-b4f3-4dc9-82e6-6b623bf84cb5.JPG)
  
- 렌더링을 할 때 백버퍼를 두고 하나의 버퍼는 사용자에게 보여주는 역할을 하고 다른 하나는 그 시간에 정보를 씀. 그 이후 변경이 끝나면 버퍼를 교체해 화면이 부드럽게 넘어가도록 보임

  - 즉, 버퍼 클래스는 현재 버퍼와 다음 버퍼를 두고, 정보를 읽을 때는 현재 버퍼에 접근함. 정보를 쓸 때는 다음 버퍼에 접근
  
  - 다음 버퍼에서 변경이 완료되면 현재 버퍼와 다음 버퍼를 교체해 현재 버퍼가 새로운 다음 버퍼가 되어 재사용
  
- 이중 버퍼의 문제점

  - 버퍼 교체 시간이 많이 걸릴 수 있음
  
  - 버퍼가 교체 중에는 두 버퍼에 접근할 수 없음
  
  - 버퍼가 두 개가 필요함
  
  (컴퓨팅 파워가 좋아지면서 무시해도 될 정도의 문제점이 됐음)
  
- 그래픽스 외의 활용

  - **변경 중인 상태에 접근할 수 있다는게 이중 버퍼로 해결하려는 문제의 핵심**
  
  - 원인은 두 가지
  
    - 다른 쓰레드나 인터럽트에서 상태에 접근하는 경우
    
    - 어던 상태를 변경한는 코드가 동시에 지금 변경하려는 상태를 읽는 경우. 즉, 물리나 인공지능 같이 객체가 서로 상호작용할 때 이와 같은 경우를 쉽게 볼 수 있음
 
## 게임 루프

- 의도: 게임 시간 진행을 유저 입력, 프로세서 속도과 디커플링
  
  (커플링이 있을 때 하나가 지연되면 줄줄이 지연돼 게임에서는 최악)
  
- Unreal에서는 게임 로직 루프와 렌더 루프로 나뉨

- 게임 루프에서 핵심은 사용자의 입력을 처리하지만 마냥 기다리고 있지 않는다는 점. 루프는 끊임없이 돌아감

  - 실제 시간 동안 게임 루프가 얼마나 많이 돌았는지 측정하면 FPS(초당 프레임 수)를 얻을 수 있음

    - 루프가 빠르게 돌면, FPS가 올라가 부드럽고 빠른 화면을 볼 수 있고 느리게 돌면, 뚝뚝 끊어져 보임

  - Frame rate를 결정하는 요인

    - 한 프레인에 얼마나 많은 작업을 하는가
    - 코드가 실행되는 플랫폼의 속도

- 패턴: 게임 루프는 게임 내내 실행됨. 한 번 돌 때마다 멈춤 없이 유저 입력을 처리한 뒤 게임 상태를 업데이트하고 렌더링함


