### Rxjava 개념

#### 리액티브 프로그래밍
- 리액티브 프로그래밍은 데이터의 변화가 발생했을 때, 변경이 발생한 곳에서 새로운 데이터를 보내 줍니다.(push)

#### 콜백?
- 콜백이나 옵서버 패턴은 스레드에 안전하지 않다
- 한두개의 스레드는 괜찮지만, 수십 수백개의 스레드가 동작하고 있는 곳에서는 디버깅하기 매우 어렵다.
- 비동기 상황에서 콜백은 필수이다
- 콜백을 많이 사용하면, 가독성이 떨어지고, 디버깅이 어려워진다.


#### 기본 예제, 람다

~~~
public class Main {
    
    public static void myPrint(String a){
        System.out.println(a);
    }
    
    public static void main(String[] args){
        Observable<String> source = Observable.just("1","2","3");
        source.subscribe(new Consumer<String>() {
            @Override
            public void accept(String s) throws Exception {
                Main.myPrint(s);
            }
        });
        
        source.subscribe(data -> myPrint(data));
        source.subscribe(Main::myPrint);
    }
}
~~~

#### Observable 클래스
- 데이터의 변화가 발생하는 데이터 소스

#### just() 함수
- Observable 클래스의 가장 단순한 선언 방식, Interger 같은 래퍼 타입 부터, 사용자 객체 까지 넣는거 가능

#### subscrube() 함수
- subscribe()함수는 Observable을 구독합니다. subscribe() 함수를 호출해야만 비로소 변화한 데이터를 구독자에게 발행합니다.


#### 마블 다이어그램
<image src="https://user-images.githubusercontent.com/23315291/45067222-25ab5500-b0fd-11e8-8820-448be4633029.png" width = "700" height="300">

- 위 실선은 시간, 시간순으로 데이터가 발행되고 있다.
- 시간 순서대로 별, 삼각형, 오각형 도형을 Observable에서 발행한다. 데이터 발행할 때는 onNext 알림이 발생한다.
- |는 데이터 발행이 완료되었다는것을 의미한다.
- X는 입력값을 처리할 때 발생한 에러

