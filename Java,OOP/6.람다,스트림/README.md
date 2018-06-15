### 람다 표현식
- 함수형 프로그래밍
    - 익명 메소드
    - 파라미터가 있는 표현식
    - 익명 내부 클래스의 다른형태
    - closure
- 간결성, 강건성을 유지하기 위해 도입
- 함수를 인자로 전달 가능
~~~
public class StringLengthComparator implements Comparator<String> {
    @Override
    public int compare(String s1, String s2){
        return s1.length() – s2.length();
    } // compare
} // StringLengthComparator
~~~
- 위 코드를 아래처럼..
- 다시 사용할 필요가 없는 경우
~~~
Arrays.sort(list, new Comparator<String>(){
    @Override
    public int compare(String s1, String s2){
        return s1.length() – s2.length();
    }
});
~~~

- 이름이 없는 코드 블록, 파라미터와 몸체로 구성됨
- **표현식이지 메소드가 아님**
- **다른 데이터처럼 인자로 전달 가능**

#### 문법
- 구성요소 : 파라미터, 코드블록, 자유변수(파라미터도 아니고, 코드 내부에서 정의되지도 않은 변수)
- (String msg) -> {System.out.println(msg);}
- msg -> {System.out.println(msg);}
- msg -> System.out.println(msg)
- String msg -> {System.out.println(msg);} // error
- (n,int d) -> (n%d == 0) //error

~~~
() -> 3.14
double myPI(){return 3.14;}
~~~
- 람다 표현식 예
~~~
@FunctionalInterface
interface StringToIntMapper{
    int map(String s);
}
~~~
~~~
StringToIntMapper mapper
    = new StringToIntMapper(){
    @Override
    public int map(String s){
        return s.length();
    }
}
String name = "cse";
int mapValue = mapper.map(name);
~~~
~~~
StringToIntMapper mapper =
    (String s) -> s.length();
String name = "cse";
int mapValue = mapper.map(name);
~~~
#### 함수형 인터페이스
- @FunctionalInterface -> 함수가 하나만 있는 인터페이스(SAM(Single Abstract Method))
- Comparable, Runnable 들이 이에 속함
- - **Object**에 정의 되어 있는 메소드의 재선언은 여러개 있어도됨.
~~~
@FunctionalInterface
interface Predicate<T>{
    boolean test(T n);
}
~~~
~~~
Predicate<Integer> isEven = n -> (n % 2 == 0);
…
System.out.println(isEven.test(n)? "짝수": "홀수");
~~~

<image src="https://user-images.githubusercontent.com/23315291/41454368-1c6660ea-70b4-11e8-9b73-0574ba1ef835.PNG" width = "450" height="300">

- 정렬.. Comparator.comparing
<image src="https://user-images.githubusercontent.com/23315291/41454527-aea6eea2-70b4-11e8-8142-e83c83bf9823.PNG" widht= "450" height="300">


### Stream
- 집단 연산(aggregate operation) : 다수 데이터에서 단일 값을 계산하는 연산 (ex.학생집합에서 평균나이)
- Stream: 순차적 또는 병행 집단 연산을 지원하는 일련의 데이터
#### Stream vs Collection
- 차이점 1. 요소를 보관하지 않는다.
- 차이점 2. 원본 데이터를 변경하지 않는다.
- 차이점 3. 무한 요소를 지원함(계속 만들어 사용 가능)
- 차이점 4. internal iteration을 활용
~~~
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum =
    numbers.stream().filter(n -> n % 2 == 1).map(n -> n*n).reduce(0, Integer::sum);
~~~
- 차이점 5. 병행 처리 가능함.
~~~
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum =
    numbers.parallelStream().filter(n -> n % 2 == 1)
        .map(n -> n*n).reduce(0, Integer::sum);
~~~
- 차이점 6. 지연연산을 지원함
    - 스트림에는 중간연산과 최종 연산이 있는데, 최종연산 없이는 중간연산은 일을 하지 않는다.

<image src="https://user-images.githubusercontent.com/23315291/41455002-723a1e06-70b6-11e8-9c32-d33679f469d8.PNG">

- 차이점7. 재사용이 가능하지 않음(한번 사용하고 끝!)
#### reduction 연산
- Stream을 최종 연산을 이용하여 단순 값으로 축약해줌
- max,min,findFirst,findAny,anyMatch
- reduce
<image src="https://user-images.githubusercontent.com/23315291/41455779-4e62bc6a-70b9-11e8-9d34-b041f24d781a.PNG">

- **이외 Stream 함수, 사용법은 api 참고하자...**


### reference
[람다와 스트림](https://rebeccacho.gitbooks.io/java-study-group/content/chapter14.html)

[람다가 이끌어 갈 모던 Java](https://d2.naver.com/helloworld/4911107)