> ### 개요
* 데이터를 클라이언트측과 서버측에서 주고 받으면서 통신을 할 때 검증은 중요한 단계이다. 또한 자주 사용된다. 클라이언트측과 서버측에서 둘다 검증을 해줘야 된다. 클라이언트 검증은 조작을 할 수 있어서 보안이 약하다. 하지만 서버측에서만 검증을하면 클라이언트측에서 시간도 걸리고 사용성이 부족하다. 그래서 클라이언트측에서도 검증을 하고 서버측에서도 검증을 하며 적절히 사용해야한다. 
  
```
[build.gradle에 의존성을 추가해준다. 후에 gradle을 refresh를 해줘야한다]

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

> ### 다양한 검증 Annotations

* `@NotNull` 프로퍼티는 널이 아니다.
* `@AssertTrue` 프로퍼티는 true 이다
* `@Size` 프로퍼티의 길이를 확인할 수 있다. 데이터타입은 String, Collection, Map, and array properties.
* `@Min` 프로퍼티의 최소 숫자를 정의할 수 있다.
* `@Max` 프로퍼티의 최대 숫자를 정의할 수 있다.
* `@Email` 프로퍼티가 이메일 형식인지 검증할 수 있다.
* `@NotEmpty` 프로퍼티가 널이 아니거나 빈값인지 검증할 수 있다. 검증 하는 데이터 타입은 String, Collection, Map or Array values 있다.
* `@NotBlank` 프로퍼티가 공백이있거나 널이 아닌지 검증 할 수 있다.
* `@Positive` and `@PositiveOrZero` 양수인지 검증할 수 있다. 0보다 커야한다.
* `@Negative` and `@NegativeOrZero` 음수인지 검증할 수 있다. 0보다 작아야한다.
* `@Past` and `@PastOrPresent` 현재보다 전 날짜인지 검증할 수 있다. Java 8에서 지원해주는 날짜도 검증 가능하다.
* `@Future` and `@FutureOrPresent` 현재보다 후 날짜인지 검증할 수 있다.

* `message = ""` message 옵션을 사용해 커스톰 메세지를 적용할 수 있다. 
  * ex: `@Notnull(message = "공백일 수 가 없습니다)` , message가 없을 시에는 라이브러리에서 제공하는 기본 메세지가 출력된다.

> ### 스프링과 같이 Bean validator 사용하기
* 스프링부트가 `spring-boot-starter-validation` 라이브러리를 넣으면 자동으로 Bean validator를 인지하고 스프링에 통합한다.
`LocalValidatorFactoryBean`을 글로벌 validator로 등록한다. 이 validator는 글로벌하게 적용히 되며 `@NotNull` 같은 어노테이션을 보고 검증을한다.
사용하려면 검증하려는 메소드안에 `parameter` 에 `@Valid 또는 @Validated`를 적용하면 된다. 만약 검증이 실패하면 `FieldError/ObjectError`를 생성해서 `BidingResult` 에 실패한 정보들을 담아준다.

```
  @PostMapping
    public ResponseEntity createItem(@Validated @ModelAttribute Item item, BindingResult bindingResult){
        return new ResponseEntity<>(itemRepository.save(item).getId(), HttpStatus.OK);
    }
```

  * `@valid`는 자바 표준 어노테이션이고 `@validated`는 스프링이 어노테이션이다. 둘 다 동일하게 작동된다.  

> ### Validate(검증)시 @ModelAttribute 와 @RequestBody차이점 
* `@ModelAttribute`는 HTTP 요청 파라미터를 다룬다.(URL 쿼리 스트링, POST Form)
* `@RequestBody`는 HTTP Body의 데이터를 객체로 변환할 떄 사용한다. 주로 API JSON 형태로 데이터를 받을 떄 사용한다.
* 검증시 둘의 차이점은`@ModelAttribute` 는 필드 단위로 정교하게 바인딩이 적용된다. 특정 필드가 바인딩 되지 않아도 나머지 필드는 정상 바인딩 되고, `Validator`를 사용한 검증도 적용할 수 있다.
`@RequestBody 는 HttpMessageConverter` 단계에서 JSON 데이터를 객체로 변경하지 못하면 이후 단계 자체가 진행되지 않고 예외가 발생한다. 컨트롤러도 호출되지 않고, `Validator`도 적용할 수 없다.

> ### 결론
* 클라이언트측에서도 검증이 필요하지만 조작해서 보낼 수 있기 때문에 보안상 서버측에서도 검증이 중요하다. 검증을 하기 위해선 `spring-boot-starter-validation` 의존성을 추가해준 객체들의 필드값에 원하는 검증
어노테이션을 달아준 후 받는 메소드에 `@Validate 나 @Valid ` 어노테이션을 사용하면 스프링에서 데이터 검증을 해준다. 이처럼 간편하게 스프링과 어노테이션을 이용해 검증을 할 수 있다.

> ### 참조
* 김영한님의 인프런강의[스프링 MVC 패턴 2]