## Java Garbage Collection
### 개요
- 객체들은 힙에 생성된다.
- Java는 프로그램 코드에서 메모리를 명시적으로 지정하여 해제하지 않는다.
    - 가끔 명시적으로 해제 하기 위해 객체를 null로 지정하거나, System.gc()메서드를 호출한다.
    - null로 지정하는것은 문제가 되지 않지만, System.gc()는 절대 사용하면 안된다!

- GC는 GC 대상이 되는 객체들을 힙에서 제거 하기 위해 JVM에서 제공된다.
- GC의 대상?
    - 모든 객체 참조가 nul일 경우
    - 객체가 블럭 안에서 생성되고 블럭이 종료 되었을때
    - 부모 객체가 null인 경우, 자식 또는 포함된 객체들은 자동적으로 GC 대상
### GC와 Reachability
- java GC는 객체가 가비지 인지 판별하기 위해 reachability라는 개념을 사용한다.
    - unreachable 객체를 가비지로 간주해 GC를 수행한다.
- 한 객체는 여러 다른 객체를 참조하고, 마찬가지로 참조된 객체는 다른 객체를 참조...
- 참조 사슬을 이룬다. -> 이런 경우 항상 유효한 최초의 참조가 있어야 하는데 이를 객체 참조의 root set 이라고 한다.

![runtime data area](https://d2.naver.com/content/images/2015/06/helloworld-329631-1.png)
- 힙에 있는 객체들의 대한 참조는 4가지 이다.
    - 힙 내의 다른 객체 참조
    - Java 스택, java 메소드 실행 시에 사용하는 지역 변수와 파라미터들에 의한 참조
    - JNI(Java Native Inteface)에 의해 생성된 객체에 대한 참조
    - 메소드 영역(클래스 정보)의 정적 변수에 의한 참조

![reachable object and unreachable object](https://d2.naver.com/content/images/2015/06/helloworld-329631-2.png)
- 위 그림처럼, root set으로 부터 시작한 참조 사슬에 속한 객체는 reachable이고, 빨간색이 unreachable 객체로 GC 대상이다.
- 파란색 참조를 보통 일반적인 참조, strong 참조라고 한다.
- 오른쪽 아래 객체처럼 reachable 객체를 참조해도, 다른 reachable 객체가 이 객체를 참조하지 않는다면 이 객체는 unreachalbe 객체이다.

### 약한 참조
- java에서는 쉽게 GC의 대상이 되도록 하기 위해 WeakReference를 제공한다.
- GC가 동작할때, unreachable 뿐만 아니라, weakly reachable 객체도 가비지 객체로 간주되어 메모리에서 회수된다.
- 이를 이용하면, 참조는 가능하지만 항상 유효할 필요는 없는 LRU 캐시(Cache)와 같은 객체들을 저장하는 구조를 쉽게 만들수 있다는데...
- WeakReference 뿐만아니라, SoftReference, PhantomReference등 여러가지 방식의 reachability를 지정할수 있다.

### 매우 한정된 메모리에서 GC..
- 모바일, 임베디드환경 에서...
- 힙에 새로운 객체를 생성할 메모리가 없다면 JVM은 OOM(Out of Memory Error)을 발생시킨다.

### reference
- [Java Garbage Collection](https://d2.naver.com/helloworld/1329)
- [Java Reference와 GC](https://d2.naver.com/helloworld/329631)
- [GC 모니터링 방법](https://d2.naver.com/helloworld/6043)
