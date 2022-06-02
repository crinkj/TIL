> ### 개요
* 데이터를 클라이언트측과 서버측에서 주고 받으면서 통신을 할 때 검증은 중요한 단계이다. 또한 자주 사용된다. 클라이언트측과 서버측에서 둘다 검증을 해줘야 된다. 클라이언트 검증은 조작을 할 수 있어서 보안이 약하다. 하지만 서버측에서만 검증을하면 클라이언트측에서 시간도 걸리고 사용성이 부족하다. 그래서 클라이언트측에서도 검증을 하고 서버측에서도 검증을 하며 적절히 사용해야한다. 
  
```
[build.gradle에 의존성을 추가해준다. 후에 refresh를 해줘야한다]

 implementation 'org.springframework.boot:spring-boot-starter-validation'
```
  

```
[예제 코드] 

  import lombok.Data;
  import org.hibernate.validator.constraints.Range;
  import javax.validation.constraints.Max;
  import javax.validation.constraints.NotBlank;
  import javax.validation.constraints.NotNull;
  
  @Data
  public class Item {
    
    private Long id;
    
    @NotBlank
    private String itemName;
    
    @NotNull
    @Range(min = 1000, max = 1000000)
    private Integer price;
    
    @NotNull
    @Max(9999)
    private Integer quantity;
    
    public Item() {
    
    }
       
    public Item(String itemName, Integer price, Integer quantity) {
        this.itemName = itemName;
        this.price = price;
        this.quantity = quantity;
    } 
}
```  
* `javax.validation` 은 표준 인터페이스고 `org.hibernate.validator` 는 validator 구현체를 사용할 때만 제공되는 검증 기능이다. 실무에선 하이버네이트가 제공해주는 검증기능을 대부분 사용한다.

> ### Validation Annotations

* `@NotNull` 프로퍼티는 널이 아니다.
* `@AssertTrue` 프로퍼티는 true 이다
* `@Size` 프로퍼티의 길이를 확인할 수 있다. 데이터타입은 String, Collection, Map, and array properties.
* `@Min` 프로퍼티의 최소 숫자를 정의할 수 있다.
* `@Max` 프로퍼티의 최대 숫자를 정의할 수 있다.
* `@Email` 프로퍼티가 이메일 형식인지 검증할 수 있다.
* `@NotEmpty` 프로퍼티가 널이 아니거나 빈값인지 검증할 수 있다. 검증 하는 데이터 타입은 String, Collection, Map or Array values 있다.
* `@NotBlank` can be applied only to text values and validates that the property is not null or whitespace.
* `@Positive` and `@PositiveOrZero` apply to numeric values and validate that they are strictly positive, or positive including 0.
* `@Negative` and `@NegativeOrZero` apply to numeric values and validate that they are strictly negative, or negative including 0.
* `@Past` and `@PastOrPresent` validate that a date value is in the past or the past including the present; can be applied to date types including those added in Java 8.
* `@Future` and `@FutureOrPresent` validate that a date value is in the future, or in the future including the present.