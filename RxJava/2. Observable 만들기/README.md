### Observable
- 데이터 흐름에 맞게 알림을 보내 구독자가 데이터를 처리할 수 있도록 합니다.
- 매우 중요한 개념...
- Observable, Maybe, Flowable

#### Observable 클래스
- 옵서버 패턴을 구현한다.
- 객체의 상태 변화를 관찰하는 관찰자 목록을 객체에 등록한다.
- 그리고 상태변화가 있을 때마다 메서드를 호출하여 객체가 직접 목록의 각 옵서버에게 변화를 알려준다.
- Observable : '현재는 관찰되지 않았지만 이론을 통해서 앞으로 관찰할 가능성을 의미한다.'

#### just() 함수
- 데이터를 발행하는 가장 쉬운 방법
- 기존의 자료구조를 이용할수 있다.

<image src="https://user-images.githubusercontent.com/23315291/45068502-b2a4dd00-b102-11e8-9015-c907ed813373.png" width = "700" height="300">

~~~
    public void emit(){
        Observable.just(1,2,3,4,5,6).subscribe(System.out::println);
    }
~~~

#### subscribe() 함수와 Disposable?
- rxjava는 just() 등의 함수로 데이터 흐름을 정의한후, subscribe() 함수를 호출하여 실행되는 시점을 조절할수 있다.
- subscribe() 함수는 Disaposable 인터페이스의 객체를 리턴 합니다.

ex)
~~~
Observable<String> source = Observable.just("1","2","3");
Disposable d = source.subscribe(v-> System.out.println("onNext() : value  " + v),
                err -> System.err.println("onError() : err : " + err.getMessage()),
                () -> System.out.println("onComplete()"));

System.out.println("isDisposed() : " + d.isDisposed());
~~~
~~~
onNext() : value  1
onNext() : value  2
onNext() : value  3
onComplete()
isDisposed() : true
~~~


#### create() 함수
- just() 는 데이터를 인자로 넣으면 자동으로 알림 이벤트가 발생하지만, create는 onNext, onComplete, onError 같은 알림을 개발자가 직접 호출해야 합니다.
- Rxjava 익숙한 사용자만 활용하도록 권고한다고 합니다.. 넘어가기


#### fromArray()
- just() 단일 데이터 발행, fromArray()는 배열 데이터 발행

~~~
Integer[] arr = {100,200,300};
Observable<Integer> source = Observable.fromArray(arr);
source.subscribe(a -> System.out.println(a));
~~~
~~~
100
200
300
~~~

- 숫자 뿐만 아니라 사용자 정의 클래스도 가능
- 그런데... int는?
~~~
int[] intArray = {400,500,600};
Observable.fromArray(intArray).subscribe(System.out::println);
~~~
~~~
I@200a570f
~~~
- int 배열의 주소가 나오는듯 하다...
- Rxjava에서 int 배열을 인식시키려면, Integer[] 로 변환 시켜 주어야 한다.


#### fromIterable()
- ArrayList, BlockingQueue, HashSet, LinkedList, Stack, TreeSet, Vector 등을 발행하도록 해준다.
- 왜냐하면, 위의 클래스는 Iterable\<E>를 구현하기 때문에...


#### fromCallable()
- 기존 자바에서 제공하는 비동기 클래스나 인터페이스와의 연동
~~~
        Callable<String> callable1 = new Callable<String>(){
            @Override
            public String call() throws Exception {
                Thread.sleep(1000);
                return "Hello Callable";
            }
        }; 

        Callable<String> callable = () -> {
            Thread.sleep(1000);
            return "Hello Callable";
        }; // 람다 표현식
        
        Observable<String> source = Observable.fromCallable(callable);
        source.subscribe(System.out::println);
~~~
- 