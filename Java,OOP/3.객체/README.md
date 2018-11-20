# 객체
- 객체 : 행위,상태,식별자를 가진다
- 클래스 : 특정 객체가 어떤 상태를 가지고 있고, 어떤 행위를 할 수 있는지 알려주기 위해 클래스를 정의함

###  overriding & overloading
- 오버로딩 : 같은 이름의 메소드가 여러개 있을수 있음
    - 파라미터의 타입, 갯수에 따라 메소드를 다르게 정의한다.
- 오버라이딩 : 부모 클래스 또는 인터페이스에 정의된 메소드를 그것을 정의한 클래스에서 재정의하는것

### 타입과 변수
- 타입 : 자바에서 사용되는 모든 값은 어떤 특정 자료 유형 또는 타입
    - ex) String 타입, System.out은 PrintStream 타입
- 변수 : 어떤 값을 저장하는 곳
    - 값이 저장 되는 곳은 주기억 장치

### 객체참조변수
- 객체를 생성하는 이유? 객체의 메소드를 호출하여 유용한 일을 하기 위해
    - 보통 한번 사용하고 버리는 것이 아니라, 여러번 사용할 경우 객체를 생성
- 생성한 객체를 유지하는 객체변수가 필요함 -> **객체참조변수**
    - 객체참조변수 : 객체를 저장하는곳(x), 객체가 저장되어 있는 주소를 저장하는곳

#### 참조 변수의 ==, != 연산
- == 와 != 는 동일한 객체를 참조하는지 알아볼때 사용한디.
- 그러므로, 동일한 객체를 참조하고 있을 겨우 == 는 true.

### 자바에서 인자전달
- primitive type : byte,char,int 와 같은 자바 기본 타입
- reference type : java.lang.object를 상속받는 모든 객체

1. primitive type이 인자로 전달되는 경우
~~~
public class test1 {
     
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        int a = 10;
        System.out.println("a = "+a);
        add(a);
        System.out.println("a = "+a);
    }
    static void add(int x) { // a의 값을 1증가 시켜주는 함수
        x++;
    }
 
}
~~~
~~~
a = 10
a = 10
~~~

- 값이 바뀌지 않는 것을 보다 Call by value 이다.

2. 객체 레퍼런스로 인자가 전달되는 경우
~~~
class Point{
    int x;
    Point(int x){
        this.x = x;
    }
}
public class test2 {
     
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Point p = new Point(10);
        System.out.println("a = "+p.x);
        add(p);
        System.out.println("a = "+p.x);
    }
    static void add(Point a) { // a.x 값을 1 증가 시켜주는 함수
        a.x++;
    }
 
}
~~~
~~~
a = 10
a = 11
~~~
- 값이 바뀌는 것을 보아 call by reference임을 알 수 있다.

3. 배열이 인자로 전달되는 경우
~~~
public class test2 {
     
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        int a[] = {10, 20};
        System.out.println("x = "+a[0]+", y = "+a[1]);
        swap(a);
        System.out.println("x = "+a[0]+", y = "+a[1]);
         
         
    }
    static void swap(int x[]) { // x[0]와 x[y] 값을 교환하는 함수
        int temp = x[0];
        x[0] = x[1];
        x[1] = temp;
    }
 
}
~~~
~~~
x = 10, y = 20
x = 20, y = 10
~~~
- 값이 바뀌는 것을 보아 Call by reference임을 알 수 있다.

- **즉, primitive type은 Call by Valude, reference type은 Call by Reference**


#### deep copy? swallow copy?
~~~
void f1(int[] a){
    int[] B = {5,6,7,8};
    a = B;
}
void f2(){
    int[] A = {1,2,3,4};
    f1(A);
    for(int i=0; i<4; i++)
        System.out.println(A[i]);
}
~~~
~~~
결과: 1,2,3,4
~~~

- 위 결과를 보면, A의 값이 변하지 않았다.
- f1의 파라미터 int[]a는 A를 참조 참조객체변수!
- 이런 참조객체변수 a는 A가 참조하는 객체 에서 B를 참조하는 객체로 참조를 바꾸는것!
- 그러므로, 결과적으로 A의 값은 변하지 않는다!

<img width="300" height="230" src="https://user-images.githubusercontent.com/23315291/41354173-3f6c0e84-6f59-11e8-84d0-49ea03fb6128.png">