## 자바 메모리 구조

### JVM
- 기존 프로그램은 os가 제어하고 있는 시스템의 리소스의 일부인 RAM을 제어 할수 있다.
- c 같은 경우 os에 종속되어 실행되지만,
- java는 JVM(Java Virtual Machine)은 os에게서 메모리 사용권한을 할당 받고, os에 독립적으로 실행된다.
- java와 os 사이에 중계자 역할을 하고, 메모리 관리 기능(Garbage Collection)

### java 메모리 구조
![java memory struct](https://t1.daumcdn.net/cfile/tistory/0139C94D51A4557F39)


### Runtime Data Area
#### 메소드 영역
- 필드 데이터, 메소드 데이터, 메소드 코드, 생성자 코드 등을 분류해서 저장한다.
- 메소드 영역은 JVM 시작할대 생성되고 모든 스레드가 공유한다.

#### 클래스 영역
![class Area](https://t1.daumcdn.net/cfile/tistory/0217134851A4559210)
- 클래스 영역 = 메소드 영역
- static area라고도 불린다.
- 클래스의 인스턴스 끼리 공유하는 메소드, static 변수등을 저장한다.

#### 스택 영역
![stack Area](https://t1.daumcdn.net/cfile/tistory/2542F84D51A455AC33)
- 메소드 호출시 메소드안의 지역변수, 메소드만의 공간을 생성한다.(Like 재귀호출..?)
- C/C++ 도 같은 구조로 되어 있다.

#### 힙 영역
![heap Area](https://t1.daumcdn.net/cfile/tistory/2360B14C51A455C823)
- new 연산자로 생성된 객체와 배열을 저장하는 공간
- 변수가 힙영역에 생성된 객체를 참조한다.
- 이때, 힙 영역에 객체가 어떤 변수의 참조를 받지 않으면, GC의 대상이 되어 메모리 반납된다.
- 잘못된 개발로 GC의 대상이 되지 않는 경우도 있다는데...

### vs C
- C는 데이터 영역, 힙 영역, 스택 영역
- java와 비슷
- 데이터 영역은 전역변수와 static 변수가 할당됨, 프로그램 시작과 동시에 할당
- 힙 영역은 런타임에 생성된 변수 저장
- 스택은 java와 같다..


### reference
- [JVM-메모리구조](http://huelet.tistory.com/entry/JVM-%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B5%AC%EC%A1%B0)