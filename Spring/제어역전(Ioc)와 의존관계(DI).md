> # 제어역전(Inversion of Control) 과 의존성 주입(Dependency Injection)

 > 제어 역전(Inversion of Control) 
* 제어역전(IoC)은 스프링 프레임워크에 코어이다.
* `org.springframework.beans` and `org.springframework.context` 패키지는 스프링 프레임워크의 IoC container를 담당하는 기초 패키지다. `BeanFactory` 인터페이스는 모든 유형의 객체를 관리하고 담당하는 메커니즘을 제공한다.  `ApplicationContext`이란 `BeanFactory`의 하위 인터페이스며, IoC를 담당하는 IoC container이다. 스프링이 제어권을 가지고있으며 직접 객체를 생성하여 의존관계를 가지고있는 객체들 `Spring Beans` 라 부른다. `Ioc Container` 가 bean들을 제어하며 생명주기를 관리한다. 

> 의존성 주입(Dependency Injection)


* 의존성 주입(DI)는 하나의 객체를 다른 객체에 연결시켜주거나/주입시켜주는 행위를 말한다. 

```
public class Store {
    private Item item;
 
    public Store() {
        item = new ItemImpl1();    
    }
}
```
- 예전에는 위와 같이 Store 클래스에 인스턴스를 주입하기위해선 클래스 내부에서 인스턴스를 생성했어야했다. 
```
public class Store {
    private Item item;
    public Store(Item item) {
        this.item = item;
    }
}
```
* 의존성주입(DI)를 사용하여, 위와같이 생성자에 의존성을 주입해서 사용할수가 있다.


> DI 장점
* 결합도를 낮춰주며 로직을 수정할 필요가 적어진다.
* Boilerplate(반복적) 코드가 줄어든다.
* 재사용성/가독성/테스트성이 증가하며 코드 퀄리티가 좋아진다.
* 시스템 안전성이 증가하며 도메인의 로직변화가 있어서 모듈에 끼치는 영향이 줄어든다.

<br>

> references <br>
 * https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring#:~:text=Dependency%20injection%20is%20a%20pattern,than%20by%20the%20objects%20themselves.
