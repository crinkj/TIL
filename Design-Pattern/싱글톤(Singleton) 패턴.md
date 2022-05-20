> ### 싱글톤 패턴 개요
* 스프링 프레임워크가 생겨난 개요는 기업용 온라인 서비스 기술을 지원하기 위해 탄생했다. 물론 웹이 아닌 다른 어플링케이션 개발도 개발을 할 수 있다. 하지만 `스프링은 대부분 웹 어플리케이션`으로 많이 사용된다. `대부분 기업용 온라인 서비스 기술`은 보통 여러 고객이 동시에 요청을 한다. 자바의 싱글톤 패턴과 스프링의 싱글톤 패턴은 하나의 인스턴스를 공유해서 사용한다는 개념은 비슷하지만 구현체에 차이점이 있다. 일단 스프링 싱글톤 패턴을 알기전에 기본 싱글톤 패턴을 알아야한다. 

  ```
    void pureContainer(){
        AppConfig appConfig = new AppConfig();

        //1. 조회: 호출할 떄 마다 객체를 생성
        MemberService memberService1 = appConfig.memberService();

        //2. 조회: 호출할 떄 마다 객체를 생성
        MemberService memberService2 = appConfig.memberService();

        //참조값이 다른 것을 확인
        System.out.println("memberService1 = " + memberService1);
        System.out.println("memberService2 = " + memberService2);

        // memberService != memberService2
        Assertions.assertThat(memberService1).isNotSameAs(memberService2);
    }

  결과) 
   memberService1 = test.core.member.MemberServiceImpl@6200f9cb
   memberService2 = test.core.member emberServiceImpl@2002fc1d
  ```
* 위에 테스트 코드를 돌렸을때 `isNotSameAs() 함수를 사용해 memberService1 과 memberService2는 다른 객체이므로 테스트가 통과되며 memberService1 과 memberService2` 는 다른 참조값을 가지고있다. 그러므로 AppcConfig는 요청할 떄마다 JVM 메모리에 새로운 객체가 올라가고 생성한다. 이런 방법은 트래픽이 많은 웹 어플리케이션에서 메모리 낭비가 심하다. 
     * 위 문제 해결책: 객체를 하나만 생성하고 공유해서 사용하면 효율적으로 개발이 가능하다 <-  `싱글톤 패턴`

> ###  싱글톤 예제
```
public class SingletonService {

    (1) private static final SingletonService instance = new SingletonService();

    (2) public static SingletonService getInstance(){
        return instance;
        }

    (3) private SingletonService(){
        }
}
```
1.  static 으로 명시해서 어플리케이션 시작과 동시에 인스턴스를 new 키워드로 생성해서 static 영역에 올려둔다.
2.  인스턴스에 사용이 필요하면 getInstance()메소드를 통해서만 조회할 수 있다. 항상 같은 인스턴스만 반환된다.
3.  딱 1 개의 인스턴스만 존재해야 하므로, private 접근제한자를 가진 기본 생성자를 통해 외부에서 new 키워드를 통해 인스턴스가 생성되는것을 막는다.
```
 @Test
    @DisplayName("싱글톤 패턴을 적용한 객체 사용")
    void singletonServiceTest(){
        SingletonService singletonService1 = SingletonService.getInstance();
        SingletonService singletonService2 = SingletonService.getInstance();

        System.out.println("singletonService1 = " + singletonService1);
        System.out.println("singletonService2 = " + singletonService2);

         assertThat(singletonService1).isSameAs(singletonService2);
    }

출력 결과:
  singletonService1 = test.core.singleton.SingletonService@23348b5d
  singletonService2 = test.core.singleton.SingletonService@23348b5d
```    
* `isSameAs() 함수를 사용해 service1과 service2는 같은 인스턴스` 이기 떄문에 테스트가 동과 되며 위에 참조값을 통해 같은 인스턴스임을 알 수 있다.
> ![imge](https://blog.kakaocdn.net/dn/bKtjYw/btrgg4mqByQ/mvCRiy9ry6OrxyLkOJBbl0/img.png)
* 위에서 구현한 코드는 위 사진과 같이 하나의 인스턴스를 static 영역에 올려놓고 getInstance()를 통해 인스턴스를 공유하며 같이 사용한다. 하지만 이렇게 하나의 인스턴스를 공유하고 사용하기위해 무조건 private 기본 생성자로 막아놓는다. 

> ### 결론
  * 싱글턴 패턴을 사용하면 객체를 효율적으로 공유해서 사용할 수 있다. 하지만 따라오는 side-effect또한 많다.
  
    * side-effect
      * 의존관계상 클라이언트가 구체 클래스(getInstance()같은)에 의존한다. -> DIP 원칙을 위반하며 OCP 원칙을 위반할 가능성이 높다.
      * 내부 속성을 변경하거나 초기화 하기 어렵다
      * private 생성자를 사용하므로 자식 클래스를 만들기 어렵다 -> 유연성이 떨어진다.
