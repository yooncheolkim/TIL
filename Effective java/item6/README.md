### 불필요한 객체 생성을 피하라
```
String s = new String("bikini); //따라 하지 말것!
String s = "bikini" 
```
- 위와 같이 코딩하면, 매번 새로운 객체를 만들어낸다.
- 아래 코딩은, JVM 내부에서 이와 같은 문자를 사용하는 모든 코드가 같은 객체를 재사용함이 보장된다.


- 생성 비용이 아주 비싼 객체도 있다.
- 이런 '비싼 객체'가 반복해서 필요하다면 캐싱하여 재사용하길 권한다.

```
static booean isRomanNumeral(String s){
    return s.matches(^((?=.)M)~~~");
}
```
- Stirng.matches는 정규표현식으로 문자열 형태를 확인하는 가장 쉬운 방법이지만, 성능이 중요한 상황에서는 반복해 사용하기는 적합하지 않다.
- 이 메서드 내부에서 사용하는 Pattern 인스턴스는, 인스턴스 생성비용이 높기 때문에, 캐싱해두고 사용하여야한다.

```
public class RomanNumerals{
    private static final Pattern ROMAN = pattern.compile(
        "^(?=.)~~~~);

    static boolean isRomanNumeral(String s){
        return ROMAN.matcher(s).matches();
    }
}
```

#### 어댑터
- 어댑터는 실제 작업은 뒷단 객체에 위임하고, 자신은 제2의 인터페이스 역할을 해주는 객체다. 어댑터는 뒷단 객체만 관리하면 된다.
- 뒷단 객체 외에는 관리할 상태가 없으므로 뒷단 객체 하나당 어댑터 하나씩만 만들어지면 충분하다.
```
Map<String, Integer> menu = new HashMap<>();
menu.put("Burger",8);
menu.put("Pizza",9);

Set<String> names1 = menu.keySet();
set<String> names2 = menu.keySet();

names1.remove("Burger");
System.out.println(names2.size());
System.out.println(menu.size());
```
- 위와 같은 경우, names1.remove 하는 순간, names2의 keySet 과 menu의 "Burger" map 요소도 사라지게 된다.
- 모두가 같은 Map 인스턴스를 레퍼런스 하기 때문이다. 


#### 오토박싱
- 프로그래머가 기본타입(primitive) 과 박싱된 기본 타입(wrapper) 를 섞어 쓸떄 자동으로 변환해주는 기술이다.
- 오토 박싱은 기본 타입과 그에 대응하는 박싱된 기본 타입의 구분을 흐려주지만, 완전 없애주지는 않는다.
```
private static long sum(){
    Long sum = 0L;
    for(long i = 0; i <= Integer.MAX_VALUE; i++)
        sum += i;    
    
    return sum;
}
```
- sum+ = i; 를 할때마다 기본 타입이 박싱된 타입으로 형변환이 계속해서 발생한다.
- 이 때문에 굉장히 많은 객체 생성이 발생하게 되고, 시간을 엄청나게 잡아먹는다.
- 불필요한 오토박싱을 피하려면 박스 타입 보다는 프리미티브 타입을 사용해야한다.


#### 참고
- 이번 아이템은 방어적 복사(defensive copy - 아이템 50)를 다루는 아이템과 대조적이다. 
- 방어적 복사를 해야하는 경우 객체를 재사용하면 심각한 버그와 보안성에 문제가 생긴다.
- 불필요한 객체 생성은 그저 코드 형태와 성능에만 영향을 준다....
- 버그와 보안성이 성능보다 중요하다...

