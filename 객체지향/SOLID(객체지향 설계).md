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
* 클래스들은 `확장에는 열려있어야하고 변경에는 닫혀`있어야한다. 기존 기능의 새로운 기능을 추가할 경우 기존 코드는 변경을 안하면서 확장할 수 있도록 코드를 짜야한다.
> ## Liskov Substitution Principal(LSP)
> ## Interface Segregation Principal(ISP)
> ## Dependency Inversion Principal(DIP)

> ## 참조
* https://www.baeldung.com/solid-principles