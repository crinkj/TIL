> # SOLID(객체지향 설계 5원칙)
> ## 개요 
* SOLID 원칙은 `객체지향적`인 디자인이다. SOLID 원칙은 개발자들이 유지보수성이 뛰어나고,가독성이 좋고, 유연성있는 소프트웨어를 개발하기 위해 필요한 원칙이다. 어플리케이션이 확장되고 커질수록 복잡도는 증가한다, 개발자는 SOLID원칙을 통해 복잡도를 감소시켜 후에 유지보수하기 좋아지며 탄탄한 어플리케이션을 만들 수 있다. 
> ## Single Responsibility Principal(SRP)
* `1개의 클래스` 는 `1개의 책임` 만 가지고 있어야한다. 그 이유는 어플리케이션이 성장하면서 클래스가 하나의 책임과 행동을 갖고 있으면 유지보수 측면에도 효율성이 향상하고 변경으로 인한 `side-effect` 가 줄어든다. 
- 장점
    * 테스트 - 테스트 케이스가 줄어든다.
    * 결합도 감소 - 한개의 책임만 가질수록 의존성이 줄어들어서 결합도가 감소된다. 
    * 정돈 - 클래스가 작아지면 작아질수록, 클래스를 정리하기 쉽고 찾기가 쉽다.
 
```
public class Book {

    private String name;
    private String author;
    private String text;

    //constructor, getters and setters

    // 택스트 변경 함수
    public String replaceWordInText(String word){
        return text.replaceAll(word, text);
    }

    public boolean isWordInText(String word){
        return text.contains(word);
    }

    // 텍스트 출력 함수
     void printTextToConsole(){
        // our code for formatting and printing the text
    }

}
```
* 위에 함수는 텍스트 변경과 텍스트를 출력하는 두개의 책임을 가지고있다. SRP 원칙에 어긋나는 클래스이다. 
  
```
public class BookPrinter {

    // methods for outputting text
    void printTextToConsole(String text){
        //our code for formatting and printing the text
    }

    void printTextToAnotherMedium(String text){
        // code for writing to any other location..
    }
}
```
* `BookPrinter` 라는 클래스를 사용해 책에 관한 출력을 책임지고 담담하는 클래스이다. 이와 같이 BookPrinter 클래스를 생성하고 분리함으로써 단일 책임 원칙을 지켜지고 있다.

> ## Open-Closed Principal(OCP, Open for Extension,Closed For Modification)
* 클래스들은 `확장에는 열려있어야하고 변경에는 닫혀`있어야한다. 기존 기능의 새로운 기능을 추가할 경우 기존 코드는 변경을 안하면서 확장할 수 있도록 코드를 짜야한다. OCP의 유일한 예외는
기존 코드의 버그를 수정할때만이다.
```
    public class Guitar{
        private String make;
        private String model;
        private int volume;
    }
```
* 기존 기타라는 어플리케이션이 있다. 기타에는 만든회사,기타종,볼륨이 있다. 모두 다 좋아했지만, 몇달뒤 실증을 느끼고 개발자는 기타에 멋있는 화염패턴을 추가하고싶어졌다.
하지만 개발자는 추가중에 어플리케이션에 에러가 생길까 무서워한다. 이럴때는 open-closed principal 원칙을 사용하면 된다.
```
public class SuperCoolGuitarWithFlames extends Guitar {

    private String flameColor;

    //constructor, getters + setters
}

```
* 위에는 open-closed principal 원칙을 따라 `SuperCoolGuitartWithFlames` 클래스를 만든 후 `기존 Guitar 클래스를 확장` 시켰다. 확장을 시키면서 기존 Guitar 클래스는 영향을 받지않음으로 에러가 발생할 확률은 없다. 이처럼 open-closed principal 원칙을 적용하면 `기능은 확장하지만 기존코드의 영향`은 안간다.
> ## Liskov Substitution Principal(LSP)
* 상위 클래스 A 와 하위 클래스 B가 있는 가정하에, 하위 클래스인 B를 상위클래스 A자리에 변경하여도 프로그램 동작에는 문제가 없어야한다.
```
public interface Car {
    void turnOnEngine();
    void accelerate();
}
```
* car 라는 `ìnterface`가 있다. 시동을 키는 함수(turnOnEngine()) 와 앞으로 가는 함수(accelerate())이 있다. 
```
public class MotorCar implements Car {

    private Engine engine;

    //Constructors, getters + setters

    public void turnOnEngine() {
        //turn on the engine!
        engine.on();
    }

    public void accelerate() {
        //move forward!
        engine.powerOn(1000);
    }
}
```
* MotorCar라는 클래스는 Car 인터페이스를 구현한다. turnOnEngine()이라는 함수는 엔지을 키고 acclerate()이라는 함수는 차의 마력을 상승시키는 함수다.

```
public class ElectricCar implements Car {

    public void turnOnEngine() {
        throw new AssertionError("I don't have an engine!");
    }

    public void accelerate() {
        //this acceleration is crazy!
    }
}
```
* ElectricCar라는 클래스는 Car 인터페이스를 구현한다. turnOnEngine()이라는 함수는 전기차이므로 엔진이 없다는 예외를 발생시킨다. 이렇게 같은 `interface`를 구현해도 프로그램 동작이 달라질경우, `Liskov substition principal` 원칙을 어긋난다. 이럴경우 해결법은 interface를 두개로 분리하고 하나의 인터페이스가 engine없는 interface로 가져가는 방법이 좋다. `Listkov substition principal`을 지키는 가장 간단한 방법은 재정의하지 않는것이다. 
> ## Interface Segregation Principal(ISP)
* 하나의 인터페이스가 여러개의 기능을 가지고 있으면 그 인터페이스는 많은 책임을 지고있다. 많은 책임을 진다는건 의존도가 강하며 인터페이스 내부가 변경시 영향끼치는 클래스들이 많다는것이다.그래서 하나의 큰 인터페이스를 여러개의 인터페이스로 쪼개면 책임이 나눠지고 관심사가 분리되면서 의존도가 감소한다. 인터페이스가 분리가되면 클라이언트는 자신이 사용하지 않는 메소드에 의존관계를 맺는걸 방지 할 수 있다.
```
public interface BearKeeper {
    void washTheBear();
    void feedTheBear();
    void petTheBear();
}
```
* 곰 지킴이라는 `interface` 가 있다. 곰지킴이를 상속받게 될경우 씻겨주고(washTheBear()), 먹여주고(feedTheBear()), 키워주고(petTheBear()) 해야한다. 먹여주고 씻겨줄수는 있지만 키워주기엔 너무 위험하다. 하지만 interface segregation 원칙을 따르면
```
public interface BearCleaner {
    void washTheBear();
}

public interface BearFeeder {
    void feedTheBear();
}

public interface BearPetter {
    void petTheBear();
}
```
* 위와 같이 인터페이스가 나눠진다. 그러면 우리는 이중에 위험한 petTheBear는 제외하고 나머지 `interface` 를 구현하면된다.
```
public class BearCarer implements BearCleaner, BearFeeder {

    public void washTheBear() {
        //I think we missed a spot...
    }

    public void feedTheBear() {
        //Tuna Tuesdays...
    }
}
```
* 위와 같이 클래스를 만든후 `BearCleaner(씻겨주고),BearFeeder(먹여주는) interface` 를 상속받으면 해결된다.
> ## Dependency Inversion Principal(DIP)

> ## 참조
* https://www.baeldung.com/solid-principles