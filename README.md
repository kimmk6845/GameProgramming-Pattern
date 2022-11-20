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

- 대량 데이터를 찍어내기 위해 사용하는 패턴 (
<u>최적화</u>
와 관련)

- 메시, 텍스처 등 공통적인 부분을 가진 인스턴스들을 많이 렌더링 해야 할 때 공통적인 부분은 묶어서 메모리를 공유시키는 방법

  - 공유시키지 않고 각 객체를 하나하나 렌더링하면 한 프레임에 GPU로 전달해야 하는 양이 너무 많음
 
  - 공유 객체 하나를 만들어 각 인스턴스들이 그 객체를 참조하는 형태로 만들고 위치나 회전, 스케일 같은 변수는 따로 지정해 줌
  
![경량](https://user-images.githubusercontent.com/71704350/202898556-f6ebb89b-f1a3-4538-85f4-1f338239c3ec.JPG)

- 공통적인 부분을 묶어 메모리를 공유하는 것이 **Flyweight**

- Flyweight의 매개변수를 한번에 처리하는 것이 **인스턴싱**. 즉, 렌더링 파이프라인에 100번의 과정을 거쳐 그려야 할 인스턴스들을 묶어서 한 번에 보내 1번의 과정으로 모두 그리는 것
