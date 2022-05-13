> # SOLID(객체지향 설계 5원칙)
> ## 개요 
* SOLID 원칙은 `객체지향적`인 디자인이다. SOLID 원칙은 개발자들이 유지보수성이 뛰어나고,가독성이 좋고, 유연성있는 소프트웨어를 개발하기 위해 필요한 원칙이다. 어플리케이션이 확장되고 커질수록 복잡도는 증가한다, 개발자는 SOLID원칙을 통해 복잡도를 감소시켜 후에 유지보수하기 좋아지며 탄탄한 어플리케이션을 만들 수 있다. 
> ## Single Responsibility Principal(SRP)
* `1개의 클래스` 는 `1개의 책임` 만 가지고 있어야한다. 
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

    // methods that directly relate to the book properties
    public String replaceWordInText(String word){
        return text.replaceAll(word, text);
    }

    public boolean isWordInText(String word){
        return text.contains(word);
    }
}
```
* 위에 코드를 보면, `Book` 이라는 인스턴스를 생성하기 위해 name,author,text라는 필드값이 필요하다.
> ## Open-Closed Principal(OCP, Open for Extension,Closed For Modification)
> ## Liskov Substitution Principal(LSP)
> ## Interface Segregation Principal(ISP)
> ## Dependency Inversion Principal(DIP)

> ## 참조
* https://www.baeldung.com/solid-principles