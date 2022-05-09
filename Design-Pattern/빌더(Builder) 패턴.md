> # 디자인 패턴: 빌더 패턴

> ## 개요

- Builder pattern 이란 디자인 패턴중에 하나로 객체지향 프로그래밍에서 객체를 생성할때 유연하게 대처하기위해 만들어진 패턴중에 하나다.

> ## 라이벌 패턴들

- Telescoping Constructor Pattern(점층적 생성자 패턴):   `<보기1>` 과 같이 피자를 생성하기 위한 기본 생성자가 있다. 만약 피자에 들어가는 내용물이 적을 경우에는 점층적 생성자 패턴을 사용하기에는 괜찮지만. 피자에 토핑이 추가된다는 가정하에 추가되면 될 수록 피 피자를 만들기는 어렵다. 이유는 토핑이 추가될수록 사용자는 재료들을 일일히 다 기억하고 들어가는 순서를 기억해서 만들어야하기 때문이다. 이러한 패턴의 문제점을 해결하기위해 나온 패턴이 JavaBeans Pattern이다.

```kotlin
Pizza(int size) { ... }        
Pizza(int size, boolean cheese) { ... }    
Pizza(int size, boolean cheese, boolean pepperoni) { ... }    
Pizza(int size, boolean cheese, boolean pepperoni, boolean bacon) { ... } 
```

                                                               `<보기1>`

- JavaBeans Pattern(자바빈즈 패턴): `<보기 2>`와 같이 객체 생성후 setter를 사용해서 객체에 값을 지정해주는 방법이다. 이 방법은 생성자를 통해 생성후 값을 setter를 통해 값을 변경하면 객체 일관성이 깨질 수 있다. 그래서 자바빈즈 패턴으로 생성할경우 immutable(변경 불가능)한 상태로 만들수가 없어서 안전성이 떨어진다.

```kotlin
data class Pizza(
	var size:Int,
  var cheese:Boolean,
  var pepperoni:Boolean,
  var bacon:Boolean
)

fun main(){
	val pizza = Pizza(10,true,true,true)
	pizza.cheese = false
}
```

                                                               `<보기2>`

> ## 빌더 패턴 장점 

- 빌더 패턴은 접층적 생성자 패턴의 안전성 + 자바빈 패턴의 가독성을 결합한 패턴이다. 빌더 패턴을 사용하면 Telescoping Constructor Pattern(점층적 생성자 패턴) 처럼 생성자에 들어가는 매개변수 순서를 외울 필요도없고 필요없는값에 null 값을 넣을필요도없다. 또한 JavaBeans Pattern(자바빈즈 패턴)와 다르게 객체 일관성이 지켜지며 변경 불가능한 상태로 생성된다.

- kotlin 에서는 기본값과 named argument(지명인자)를 제공해서 빌더 클래스를 따로 구현할 필요가 없고 밑에 보기와같이 필요한값만 지정해서 넣어서 사용하면 된다. named arguemnt(지명 인자)는 함수를 호출할 때 각 매개변수의 값만 전달하는 것이 아니고 매개변수 이름과 값을 동시에 전달할 수 있다. 그래서 순서와 무관하게 값이 전달된다.(코틀린 짱)

```kotlin
data class Pizza(
	val size:Int,
  val cheese:Boolean?=null,
  val pepperoni:Boolean?=null,
  val bacon:Boolean?=null
)

fun main(){
										// size = 10 이라고 지명인자 명시되어있다.
		val Pizza = Pizza(size = 10)
}
```

- 만약 자바인 경우에는 `<보기 3>` 과 같이 빌더를 만든 후 `<보기 4>` 와 같이 필요한 값만 객체에 적용하여 사용할 수 있다.

```kotlin
public class Pizza {
  private int size;
  private boolean cheese;
  private boolean pepperoni;
  private boolean bacon;

  public static class Builder {

    private final int size;

    private boolean cheese = false;
    private boolean pepperoni = false;
    private boolean bacon = false;

    public Builder(int size) {
      this.size = size;
    }

    public Builder cheese(boolean value) {
      cheese = value;
      return this;
    }

    public Builder pepperoni(boolean value) {
      pepperoni = value;
      return this;
    }

    public Builder bacon(boolean value) {
      bacon = value;
      return this;
    }

    public Pizza build() {
      return new Pizza(this);
    }
  }

  private Pizza(Builder builder) {
    size = builder.size;
    cheese = builder.cheese;
    pepperoni = builder.pepperoni;
    bacon = builder.bacon;
  }
}
```

                                                          `<보기 3>`

```kotlin
Pizza pizza = new Pizza.Builder(12)
                       .cheese(true)
                       .pepperoni(true)
                       .bacon(true)
                       .build();
```

                                                          `<보기 4>`

>## 결론

- Builder pattern은 주로 매개변수가 많을때 필요한 데이터만 설정할 수 있어서 가독성을 확보하고 새로운 변수가 추가된다해도 유연하게 대처할 수 있다. 또한 Setter를 사용하지않아서 확장 가능성을 닫아서 불변성을 확보할 수 있다.

> **참조**

- [https://stackoverflow.com/questions/328496/when-would-you-use-the-builder-pattern](https://stackoverflow.com/questions/328496/when-would-you-use-the-builder-pattern)
- [https://ifcontinue.tistory.com/7](https://ifcontinue.tistory.com/7)
- [https://medium.com/@vicidroiddev/using-builders-in-kotlin-data-class-e8a08797ed56](https://medium.com/@vicidroiddev/using-builders-in-kotlin-data-class-e8a08797ed56)
