> ### JVM,JDK,JRE

  ![JVM,JDK,JRE](https://user-images.githubusercontent.com/32498739/176196786-628a86af-b9d3-4bdb-9c10-77b1049f466d.png)

  

> ### JRE(JAVA Runtime Environment) 란? 
* 자바 어플리케이션을 실행할 수 있게 도와준다. 
* JVM과 핵심 라이브러리 및 자바 런타임 환경에서 사용하는 프로퍼티 세팅이나 리소스 파일을 제공한다.


> ### JDK(JAVA Development Kit) 란? 
* JRE + 개발에 필요한 툴
* 오라클에서 자바 11 버전부터는 JDK만 제공하고 JRE는 따로 제공하지 않는다. 하지만 oracle JDK 11 은 유료이기 때문에 OPEN JDK를 무료로 사용할 수 있다. 

> ### JVM(JAVA Virtual Machine) 이란? 
```
  testJava.class
---------------------------------------------------
  public class testJava{
      public static void main(String[] args){

      }
  }
  ```
* `testJava.class` 라는 클래스가 있으면 안에 있는 내용(.class 파일)은 `bytecode`라 한다. `자바 가상 머신(JVM)`은 이 bytecode를 인터프리터와 컴파일러를 통해 OS(operation system)에 특화된 코드로 변환하여 실행한다. 보통 우리가 위 클래스에서 보이는 내용은 디컴파일되서 사람이 보고 이해할 수 있다.

