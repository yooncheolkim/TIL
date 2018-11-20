### private 생성자나 열거 타입으로 싱글턴임을 보증하라.

```
public class Elvis{
    public static final Elvis INSTANCE = new Elvis();
    private Elvis(){}

    public void leaveTheBuilding(){}
}
```
- private 생성자는 Elvis.INSTANCE 초기화 할 때 한번만 호출된다.
- 클라이언트는 접근할수 있는 방법이 없다. (리플렉션 API를 사용하면 가능하다고는 한다...)

#### static 팩터리 메소드 방법
```
public class Elvis{
    private static final Elvis INSTANCE = new Elvis();
    private Elvis(){}
    public static Elvis getInstance() {return INSTANCE;}

    public void leaveTheBuilding(){}
}
```
- 위 코드와 다른점은 API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다는 점이다.
- 호출하는 스레드마다 다른 인스턴스를 넘겨줄수 있다. -> getInstance 에서 변경


#### serializable 문제
- 위 두가지 방법은 직렬화 할때 문제가 생긴다.
- 역직렬화 할때 readResolve 라는 메소드를 내부적으로 호출하는데, 이 메소드가 생성자에 직접 접근하여, 새로운 인스턴스가 생긴다.
- 그래서 가짜 Elvis 탄생을 예방하고 싶다면, 다음 과 같이 해야한다.
```
private Object readResolve(){
    return INSTANCE;
}
```

#### 열거 타입 방법
- 열거 타입이 싱글턴을 만드는 가장 좋은 방법이라고한다.
- 단, 싱글턴이 enum 외의 클래스를 상속해야 한다면 이 방법은 사용할 수 없다.
- 먼저 enum에 대한 공부를 먼저....