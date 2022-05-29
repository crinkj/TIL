> ### 개요 
* 의존성 주입(Dependency Injection)은 객체지향적으로 설계하기 위해 서로의 의존성을 주입해서 하나의 프로덕트를 만들기 위해 사용되는 중요한 개념이다. 그 중에도 의존성 주입은 대표적으로 4가지 방법이 있다. 한번 알아보자 
    * 생성자 주입: 생성자를 통해 의존성을 주입
    * 수정자 주입: Setter를 사용해 의존성을 주입
    * 필드 주입: 필드를 사용해 의존성 주입

> ### 생성자 주입
 ```
 @Autowired 
 public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy){
           this.memberRepository = memberRepository;
           this.discountPolicy = discountPolicy;
}
```
* 생성자 주입이 시작전 좋은 개발 습관이란 `개발을 할때 조작할 수 있게 열어두지말고 제약이 있고 한계를 정하는게 좋은 습관`이다. 생성자 주입 같은 경우는 위와같이 생성자를 통해 의존관계를 주입받는 방법이다. 위와 같이 의존성을 주입을 하면 생성자를 통해 호출할때 딱 1번만 호출이됨으로써 생성 된 후 변하지가 않는다. 불변하므로 호출 후 변하지 않는다는 제약이 있다. 또한 parameter가 필수임으로 개발자가 만약 parameter를 제외하고 외부에서 호출하려면 컴파일 시점에서 문제점을 확인해서 해결할 수 있다. 또한 생성자 주입은 빈 등록 하려면 생성자를 통해 생성하고 스프링 컨테이너에 등록하기 때문에 생성자를 호출해서 생성할때 의존성이 자동으로 주입된다. 

  * 무조건 클래스가 Spring bean(클래스가 @Component, @Bean 등 스프링 어노테이션을 가지고있는 클래스)이라는 가정하에 생성자가 하나일때는 위에 @Autowired를 생략해도 자동 주입된다. 두개 이상일때는 무조건 @Autowired를 붙어줘야한다.

> ### 수정자 주입
```
 @Autowired 
 public void setMemberRepository(MemberRepository memberRepository){
           this.memberRepository = memberRepository;
 }

 @Autowired 
 public void setDiscountPolicy(DiscountPolicy discountPolicy){
           this.discountPolicy = discountPolicy;
 }
```
* 수정자 주입을 통해 의존관계가 주입되는 경우는 스프링 컨테이너를 생성 후 스프링 빈을 전부 컨테이너에 등록한 후 그 후에 @Autowired를 가지고있는 객체들을 찾은 후 자동 주입한다. 단점은 변경 가능하기 때문에 어느 시점에서 개발자가 원하지 않아도 변경될 수 있는 위험성이 있다. 자바 빈 프로퍼티 규약을 따르면 캡슐화를 위해 직접 객체를 변경하지않고 위처럼 세터를 사용해 변경하기를 권장한다. 

> ### 필드 주입
```
 @Autowired 
 private MemberRepository memberRepository;

 @Autowired
 private DiscountPolicy discountPolicy;

```
* 위와 같이 객체위에다 `@Autowired` 달아주는 방식이 필드 주입이다. 이 방법은 코드가 짧고 단순해서 내가 개발할때도 많이 사용한 방법이다. 하지만 이 방법은 안 좋은 방법이다. 그 이유로는 일단 외부(예시: 테스트 코드작성시) 에서 호출을 했을떄 변경이 불가능 하다. 또한 스프링 DI 프레임워크에 너무 의존을 하고있다. 스프링 DI 프레임워크가 없으면 사용을 할 수 없다. 

> ### 결론
* 스프링을 포함한 DI 프레임워크 대부분이 생성자 주입을 권한다. 나도 평소에는 필드 주입을 사용했지만 생성자 주입을 사용해야겠다. `왜?` 
  
  1. 불변성: 의존관계 주입은 어플리케이션 시작시 생성자 호출 해서 생성된 후 종료시점까지 의존관계 변경할 일이 없다, 아니 변하면 안된다. 
  2. 수정자 주입을 사용하면 Setter를 퍼블릭으로 열어야함으로 변경 위험이 있다. 


        ![테스트 코드에서 호출](https://user-images.githubusercontent.com/32498739/170851091-ca709b23-8f34-4d56-9579-2ff68277b104.png)

        * 위와 같이 생성자 주입을 사용해 `외부(테스트 코드)`에서 호출을 하면 생성하기위해 어떤 의존성 주입이 필요한지 알려준다. 또한 `private final MemberRepository memberRepository`같이 `final` 키워드를 사용하면 생성자에 필수로 parameter가 들어가야하는 걸 알려줌으로써 생성자를 만들때 누락된 의존성을 알려준다. 수정자주입인 Setter를 사용하는 경우에는 Setter함수를 사용해서 해야하고 어떤 의존성을 주입해야하는지 알기가 어렵다. 또한 필드 주입경우는 외부에서 변경이 불가하다. 또한 DI에 의존하지 않고 순수한 자바코드로 테스트를 구현할 수 있다. 또한 필드주입을 통해 의존성 주입을 한 상태에 A라는 서비스에서 B를 호출하고 B라는 서비스에서 A를 호출하면 어플리케이션이 시작하고 순환참조한후 스택오버플로우로 서버가 죽는다. 하지만 생성자 주입같은경우 생성할 떄 의존성 주입을 서로 찾는다. 그러므로 어플리케이션 시작 전에 오류가 발생되서 개발자가 순환 참조를 방지하게 도와준다. 반면에 수정자 주입과 필드주입은 빈이 등록된후 의존성 주입을 서로 참조하기 때문에 서버 구동후 에러가 난다.  

> ### 참조
* 김영한님의 인프런강의[스프링 핵심원리 기본]