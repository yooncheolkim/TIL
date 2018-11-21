### 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라
```
public class Spellchecker{
    private static final Lexicon dictionary = ~~;

    private SpellChecker(){}

    public static boolean isValid(String word){}
    public static List<String> suggestions(String typo){}   
}
```
- 위와 같은 경우 Lexicon dictionary를 이용하여 isValid, suggestions 와 같은 메소드를 수행한다.
- 이럴때, dictionary = new KoreanDicionary(); 와 같이 고정되어있는경우 유연하지 못하다.

- 싱글톤을 잘못 사용한 예제
```
public class SpellChecker{
    private final Lexicon dictionary = ~~;
    
    private SpellChecker(){}
    public static SpellChecker INSTANCE = new SpellChecker();

    public static boolean isValid(String word){}
    public static List<String> suggestions(String typo){}   
} 
```
- 하나의 사전으로 모든 쓰임에 대응한다는것은 너무 순진한 생각이다.
- 사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글턴 방식이 적합하지 않다.


#### 의존 객체 주입은 유연성과 테스트 용이성을 높여준다.
```
public class SpellChecker{
    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary){
        this.dictionary = Objects.requireNonNull(dictionary);    
    }

    public boolean isValid(String word){}
    public List<String> suggestions(String typo){}
}
```
- 생성자를 이용하여 어떤 자원이든 의존 관계에 상곤없이 잘 작동한다.
- 의존 객체 주입은 생성자, 정적 팩터리, 빌더, 세터 등 똑같이 응용할 수 있다.


#### 생성자에 자원 팩토리를 넘겨주는 방식
- 팩터리란 호출할 때마다 특정 타입의 인스턴스를 반복해서 만들어주는 객체를 말한다. -> GoF의 팩터리 메서드 패턴
- 자바 8에 소개된 Supplier<T\> 인터페이스가 이를 완벽하게 표현한 예라고 한다....
```
Mosaic create(Supplier<? extends Tile> tileFactory){}
```

- dagger, guice, spring 같은 의존 객체 주입 프레임워크를 사용하면 코드를 간결하게 만들수 있다.