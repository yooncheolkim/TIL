### Object 클래스
- java에서 다른 클래스를 명백하게 상속받지 않는 경우, 자동으로 Object 클래스를 상속받음
- 범용 프로그래밍할때 매우 유용함
#### Object 클래스 제공 메소드
- public String toString()
- public boolean equals(Object other)
- public int hashCode()
- proteted Object clone()
- public fianl Class getClass()

#### equals 메소드
- 두 객체의 내용이 같은지, 동등성(equality)를 비교하는 메소드
- 두 객체가 같은 객체인지, 동일성(identity)를 비교하는 메소드는 hashCode()
- 파라미터 Object other을 해당 클래스로 변환해야함.
~~~
public boolean equals(Object other){
    if(other==null || getClass() != other.getClass())
        return false;
    if(this == other) return true;
    Date o = (Date)other;
    return year==o.year&&
        month==o.month&&
        day==o.day;
} // equals
~~~
- 변환 하는 과정에서, 잘못된 객체가 올수 있다.
- 이때, instanceof 로 검사가능 -> 하지만, instanceof는 하위 클래스도 오류가 발생하지 않으므로,
- getClass() 사용하는것이 좋다.

#### instanceof vs getClass()
<image src = "https://user-images.githubusercontent.com/23315291/41393382-7b688ab6-6fe0-11e8-8da9-f3803d0c3395.PNG" height ="300" width = "450">


#### hashCode() and eqauls()
- hashCode는 객체의 주기억장치 주소를 이용하여 계산됨.
- Interger , String 같은 경우 멤버 값이 같으면 hashCode가 값은 값이 나오도록 overriding 되어 있지만,
- Object 의 hashCode는 쓰레기 값을 반환 해준다.
- 즉, 사용자 정의 클래스의 두 객체의 멤버 변수가 같더라도, hashCode는 제각각이다.
- x.equals(y) 가 true이면, x.hashCode() == y.hashCode() 역시 true 이어야 한다.
- 그렇기 때문에, equals같은 메소드를 오버라이딩 할때, hashCode를 반드시 오버라이드 해야 한다.
~~~
예)
public int hashCode(){
    Integer y = Integer.valueOf(year);
    Integer m = Integer.valueOf(month);
    Integer d = Integer.valueOf(day);
    return y.hashCode()^m.hashCode()
        ^d.hashCode();
} // Date::hashCode();
~~~


#### clone 메소드
- 상태가 같은 또 다른 객체를 생성할 때 사용하는 메소드
- 규칙 1. x.clone() != x  -> 새 객체의 생성이 필요하다.
- 규칙 2. x.clone().getClass() == x.getClass() -> 동일한 클래스의 인스턴스이어야 한다.
- 규칙 3. x.clone().equals(x) == true

- Object의 clone 메소드의 문제점
    - proteced 메소드
    - 객체를 자동으로 생성하고, 인스턴스 필드의 값을 자동으로 복사함(swallo copy)
        - 클래스의 멤버 변수 중에 **참조타입**이 있으면 문제가 됨.
- 그래서, clone을 오버라이드 하는 경우 Cloneable 인터페이스를 구현!
~~~
public class B implements Cloneable{
    private int n;
    private A a;
    …
    public B clone(){
        try{
            B cloned = (B)super.clone();
            cloned.a = (A)a.clone();
            return cloned;
        }
        catch(CloneNotSupportedException e){
            return null; // can’t happen
        }
    }
}
~~~ 
public class B implements Cloneable{
    …
    public B clone(){
        throws CloneNotSupportException {
            B cloned = (B)super.clone();
            cloned.a = (A)a.clone();
            return cloned;
        }
    }
}
~~~