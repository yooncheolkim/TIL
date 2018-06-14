### interface
#### 다형성
~~~
Flyable x;//변수 타입이 interface
Flyable x = new Flyable(); // ERROR
x = new Airplane();
x.fly(); // x를 이용하여, interface에 정의된 메소드 호출 가능
~~~

~~~
Flyable x = new Bee();
Flyable y = new Parrot();
x.fly(); // ok
x.sting(); // error
~~~
- interface 타입의 변수 x를 이용해서는 interface에 정의된 메소드만 호출 가능하다.
- interface의 상수는 자동으로 **public static final**

#### 범용 정렬

~~~
public class GenericSorter{
    public static void sort(int[] array){
    …
    } // sort
} // class GenericSorter
~~~
- 위 코드의 문제는? 
    - 정수 타입의 배열만 정렬가능.
- 해결책! **template** 사용
~~~
public class GenericSorter{
    public static <T> void sort(T[] array){
    …
    } // sort
} // class GenericSorter
~~~
- 그러나, template를 사용해도, 공통 연산의 제공 필요성 때문에 **interface**개념이 필요하다!

#### template, Object 클래스
- Template 기능이 제공되기 전에는 Object를 범용 자료구조나 범용 메소드를 만들 때 많이 사용됨
- Object를 이용하여 sorting 하는 경우
- 정렬하고자 하는 객체가 compareTo 메소드를 제공하지 않을 수 있음
    - 처리할 데이터들이 어떤 특정 메소드를 제공해야 한다!
~~~
public interface Comparable<T>{
    int compareTo(T other); // Java 5
} // interface Comparable
~~~
~~~
class BankAccount implements Comparable<BankAccount>{
    public int compareTo(BankAccount other){ // Java 5
        if(this==other) return 0;
        return balance – other.balance;
    } // compareTo
    …
} // class BankAccount
~~~

#### instanceof 
- 어떤 객체가 어떤 클래스의 인스턴스인지를 검사해줌.
- B클래스가 A의 자식 클래스 이면 B 타입의 객체 b에 대해 b instanceof A의 결과는 true임


#### Builder(내부클래스)
- android 개발하다보면, 많이 보이는 Builder
- 생성자가 받아야 하는 매개변수가 많을 경우 사용할 수 있는 기법
~~~
public class Time{
    private final int hour;
    private final int minute;
    private final int second;
    public static class Builder{
        private int hour = 0;
        private int minute = 0;
        private int second = 0;
        public Builder(){}
        public Builder hour(int h){
            hour = ((h>=0 && h<24) ? h : 0); return this;
        }
        public Builder minute(int m){
            minute = ((m>=0 && m<60) ? m : 0); return this;
        }
        public Builder second(int s){
            second = ((s>=0 && s<60) ? s : 0); return this;
        }
        public Time build(){ return new Time(this); }
    } // Builder

    private Time(Builder builder){
        hour = builder.hour;
        minute = builder.minute;
        second = builder.minute;
    }
} // class A
~~~