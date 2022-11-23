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
