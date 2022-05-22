> ### 개요
* [싱글톤 컨테이너](https://github.com/crinkj/TIL/blob/master/Spring/%EC%8B%B1%EA%B8%80%ED%86%A4%20%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88.md) 에 나와 있는 내용을 보면 보통 수동으로 빈은 등록할때는 `@Bean` 을 사용하여 빈을 컨테이너에 등록 시켰다. 하지만 이런 방법은 등록하고 공통으로 사용할 빈이 많아지면 많아질수록 등록하기도 힘들어지고 관리하기도 힘들어진다. 그래서 스프링에서는 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 `@ComponentScan`이라는 기능을 제공한다. `@Autowired`를 사용해 의존관계를 자동으로 주입하는 기능도 제공한다.

```
@Configuration
@ComponentScan(
        excludeFilters = @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = Configuration.class)
)
public class AutoAppConfig {
}
```
* 위와 같이 `@ComponentScan`을 사용하면 어노테이션이 붙은 클래스들을 스프링 컨테이너에 등록시켜 사용하게해준다. 또한 `excludeFilters` 옵션을 사용해서 등록하기전 스캔할때 제외시킬 클래스를 지정해줄 수 있다. 위에 예시는 `@Configuration`이 달려있는 클래스를 제외시켰다. 기존에 `@Configuration` 를 사용해서 수동적으로 빈을 만들었기 때문에 예제로 제외시키는 옵션을 보여주기 위해 수동적으로 등록한 빈 클래스 파일을 제외시켰다.   

```
수동 빈 등록: 

@Configuration
public class AppConfig {
    @Bean
    public MemberService memberService() {
        return new MemberServiceImpl(getMemberRepository());
    }
```
* 위와 같은식으로 `@Bean` 을 사용해서 수동으로 빈을 등록했다.
  
```
자동 빈 등록:

@Component
public class MemberServiceImpl implements MemberService{
}
```
* 위와 같은식으로 `@Component`만 달아주면 자동으로 빈이 생성된다. 하지만 수동으로 등록을 할경우에는 return 할때 MemberServiceImpl(getMemberRepository()) 이렇게 MemberServiceImpl() 안에 getMemberRepository()를 파라미터로 넣어주고 리턴해줌으로써 의존성을 주입했다 하지만 `@Component`로 자동으로 등록할땐 어떻게 의존성 주입을 해야할까?
    * `@Autowired` 를 사용하자!
    
    <br>
  
  
    ```
        자동 빈 등록:

        @Component
        public class MemberServiceImpl implements MemberService{
            private final MemberRepository memberRepository;

            @Autowired
            public MemberServiceImpl(MemberRepository memberRepository){
                this.memberRepository = memberRepository;
            }
        }
    ```
    * `@Autowired`를 사용하면 스프링이 생성자 안에있는 parameter를 스프링 빈으로 주입해준다. 마치 밑과 같은식으로 수동으로 불러와 의존성 주입해주는 것처럼 자동으로 생성자 만들때 parameter도 자동으로 의존성을 주입해준다. `(주의: 해당 parameter안에있는 클래스 의존성을 주입해주기 위해서 해당 클래스도 @Component 를 사용해 빈으로 등록해줘야 의존성 주입을 할 수 있다.)`

        ```
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberService memberService1 = ac.getBean(MemberRepository.class);
        ```

> ### 결론
* `@ComponentScan` 은 `@Component`가 붙은 모든 클래스들을 스캔 후 스프링 빈으로 등록한다. 이때 스프링 빈의 기본이름은 클래스명이되 맨 앞글자만 소문자이다. `(예시: MemberServiceImpl -> memberServiceImpl 이 bean의 이름이다.) 하지만 @Component("testBean") 이라고 지정할경우 testBean 으로 bean이 등록된다. 이처럼 이름도 지정할 수 있다.` 
* `@Autowired` 를 달면 알아서 해당 빈을 찾은다음 의존성을 주입해준다. 
* `@Component` 와 `@Autowired` 를 통해 자동으로 빈이 등록되고 의존성주입도 자동으로해준다. 수동으로 할시에는 반복적으로 일일이 등록을 해줘야하는데. 자동으로 등록함으로써 많은 시간이 절약되며 효율적으로 스프링을 사용해서 개발을 할 수 있다.

> ### 참조
* 김영한님의 인프런강의[스프링 핵심원리 기본]
