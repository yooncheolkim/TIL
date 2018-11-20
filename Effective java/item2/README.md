### 생성자에 매개변수가 많다면 빌더를 고려하라.
- 생성자에 매개변수가 많으면 클라이언트는 어떻게 값을 넣어야 하는지 무척 힘들다.

```
public NutritionFacts(int servinSize, int servings)
public NutritionFacts(int servinSize, int servings, int calories)
public NutritionFacts(int servingSize, int servings, int calories,int fat, int sodium)
```
- 똑똑한 ide를 사용하면, 어떤 매개변수인지 미리 알수 있지만, 그렇지 않으면, 읽기도 힘들고, 작성하기도 힘들다.
- 이런 문제의 대안으로 자바빈즈 패턴
#### 자바빈즈 패턴
```
public NutritionFacts(){}
public void setServingSize(int val){servingSize = val;}
public void setServings(int val){servings = val;}
public void setCalories(int val){calories = val;}
public void setFat(int val){fat = val;}
public void set Sodium(int val){sdium = val;}
```
- 코드가 길어지긴 했지만, 더 읽기 쉬운 코드가 되었다.
- 하지만, 여전히 문제점이....
- 하나의 객체를 만들기 위해 여러 메서드를 호출해야하고, 완전히 객체가 완성되기 전에는 일관성(consistency)이 무너진 상태에 놓인다.
- 이런 일관성이 무너지기 때문에 클래스를 불변으로 만들수 없다.
- 이런 단점을 완화하고자 freeze(자바 스크립트에 있는듯...)를 사용하지만, 다루기 어려워서 거의 쓰이지 않는다.

#### 빌더패턴
```
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        // 필수 매개변수
        private final int servingSize;
        private final int servings;

        // 선택 매개변수 - 기본값으로 초기화한다.
        private int calories      = 0;
        private int fat           = 0;
        private int sodium        = 0;
        private int carbohydrate  = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings    = servings;
        }

        public Builder calories(int val)
        { calories = val;      return this; }
        public Builder fat(int val)
        { fat = val;           return this; }
        public Builder sodium(int val)
        { sodium = val;        return this; }
        public Builder carbohydrate(int val)
        { carbohydrate = val;  return this; }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize  = builder.servingSize;
        servings     = builder.servings;
        calories     = builder.calories;
        fat          = builder.fat;
        sodium       = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }

    public static void main(String[] args) {
        NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
                .calories(100).sodium(35).carbohydrate(27).build();
    }
}
```
- 빌더의 세터 메서드들이 빌더 자신을 반환하기 때문에 연쇄적으로 호출할 수 있다. -> 플루언트 API, 메서드 연쇄

#### 빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기에 좋다.
- 추상 클래스는 추상 빌더, 구체 클래스는 구체 빌더를 갖게 한다.
```
public abstract class Pizza {
    public enum Topping { HAM, MUSHROOM, ONION, PEPPER, SAUSAGE }
    final Set<Topping> toppings;

    abstract static class Builder<T extends Builder<T>> {
        EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);
        public T addTopping(Topping topping) {
            toppings.add(Objects.requireNonNull(topping));
            return self();
        }

        abstract Pizza build();

        // 하위 클래스는 이 메서드를 재정의(overriding)하여
        // "this"를 반환하도록 해야 한다.
        protected abstract T self();
    }
    
    Pizza(Builder<?> builder) {
        toppings = builder.toppings.clone(); // 아이템 50 참조
    }
}
```
- <T extends Builder\<T>>  : 재귀적 제네릭 타입
- self를 이용하여 하위 클래스에서 형변환하지 않고도 메서드 연쇄를 지원할 수 잇다.
```
// 코드 2-5 뉴욕 피자 - 계층적 빌더를 활용한 하위 클래스 (20쪽)
public class NyPizza extends Pizza {
    public enum Size { SMALL, MEDIUM, LARGE }
    private final Size size;

    public static class Builder extends Pizza.Builder<Builder> {
        private final Size size;

        public Builder(Size size) {
            this.size = Objects.requireNonNull(size);
        }

        @Override public NyPizza build() {
            return new NyPizza(this);
        }

        @Override protected Builder self() { return this; }
    }

    private NyPizza(Builder builder) {
        super(builder);
        size = builder.size;
    }

    @Override public String toString() {
        return toppings + "로 토핑한 뉴욕 피자";
    }
}
```
```
// 코드 2-5 칼초네 피자 - 계층적 빌더를 활용한 하위 클래스 (20~21쪽)
public class Calzone extends Pizza {
    private final boolean sauceInside;

    public static class Builder extends Pizza.Builder<Builder> {
        private boolean sauceInside = false; // 기본값

        public Builder sauceInside() {
            sauceInside = true;
            return this;
        }

        @Override public Calzone build() {
            return new Calzone(this);
        }

        @Override protected Builder self() { return this; }
    }

    private Calzone(Builder builder) {
        super(builder);
        sauceInside = builder.sauceInside;
    }

    @Override public String toString() {
        return String.format("%s로 토핑한 칼초네 피자 (소스는 %s에)",
                toppings, sauceInside ? "안" : "바깥");
    }
}
```

```
// 계층적 빌더 사용 (21쪽)
public class PizzaTest {
    public static void main(String[] args) {
        NyPizza pizza = new NyPizza.Builder(SMALL)
                .addTopping(SAUSAGE).addTopping(ONION).build();
        Calzone calzone = new Calzone.Builder()
                .addTopping(HAM).sauceInside().build();
        
        System.out.println(pizza);
        System.out.println(calzone);
    }
}
```
- 시간이 지날수록 매개변수가 많아짐에 따라, 빌더패턴은 더 효과적일것이다.
