### 프록시

#### 프록시 패턴

<img src = "https://t1.daumcdn.net/cfile/tistory/99A46433599FE0A41E?download" width = 650 >

- 프록시와 리얼 서브젝트가 공유하는 인터페이스가 있고, 클라이언트는 해당 인터페이스 타입으로 프록시를 사용한다.
- 사용 과정 : 클라이언트-> 프록시 -> 리얼서브젝트
- 클라이언트는 프록시를 통해 리얼 서브젝트를 사용하기 때문에, 프록시를 조작하면, 부가기능(접근 제한, 로깅, 트랜잭션 등)을 넣을 수 있다.


#### 다이나믹 프록시
- 다이나믹 프록시 : 런타임에 특정 인터페이스를 구현하는 클래스 또는 인스턴스를 만드는 기술
- 프록시 인스턴스 : Proxy.newProxyInstance(ClassLoader, Interfaces, InvocationHandler)

~~~java
BookService bookService = (BookService) Proxy.newProxyInstance(BookService.class.getClassLoader(), new Class[]{BookService.class},
        new InvocationHandler() {
            BookService bookService = new DefaultBookService();
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                if (method.getName().equals("rent")) {
                    System.out.println("aaaa");
                    Object invoke = method.invoke(bookService, args);
                    System.out.println("bbbb");
                    return invoke;
                }

                return method.invoke(bookService, args);
            }
        });
~~~

- 이렇게 하면, bookService의 rent 메소드를 호출할경우, aaaa bbbb를 위아래로 프린트한다.