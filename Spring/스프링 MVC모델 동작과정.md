> # 스프링 MVC 동작과정
     Servlet/Dispatcher Servlet 
 1.  Servlet: 자바 기반 웹 어플리케이션 프로그래밍 기술로서 자바 기반으로 클라이언트가 요청(Request)을하면 응답(Response)을 할 수 있게 통신하도록 도와주는 역할을 한다.


 2.  Dispatcher Servlet: Servlet은 url 매핑을 하기 위해선 web.xml에 등록을 해야했다. 또한 매번 url을 등록해줘야했기 때문에 의존성과 불필요한 반복작업들이 많다. 하지만 dispatcher Servlet은 `@Controller ` 어노테이션을 붙혀 간단히 사용할 수 있으며 스프링에서 관리해주기 때문에 의존성이 낮아지며 사용하기 쉬워진다. Dispatcher Servlet은 `FrontController(모든 리소스의 요청을 처리하는 대표 컨트롤러 패턴)`로서 Controller로 향하는 요청(Request)과 응답(Response)를 처리한다. Dispatcher Servlet을 이용한다는 말은 스프링의 MVC모델을 사용한다는 것이다. 
<br>


    DispatcherServlet 과정
   1. 스프링 어플리케이션 실행 -> DispatchServlet 등록 -> 컴포넌트 스캔(빈 등록할 클래스들 스캔) -> @RequestMapping 어노테이션을 가지고있는 핸들러를 찾아서 매핑
  
   2. 클라이언트가 요청 -> HandlerMapping에서 요청과 핸들러 매핑 -> 없을시 noHandlerFound() 메소드 실행하며 예외 던짐

   
   > ![image](https://i.stack.imgur.com/nRDbB.png)
   
     스프링 MVC 동작 과정


   > ![imge](https://terasolunaorg.github.io/guideline/5.3.0.RELEASE/en/_images/RequestLifecycle.png)
<br>
   1.  DispatcherServlet이 클라이언트에서 요청을 받는다.
 <br>  
   2.  DispatcherServlet은 핸들러 매핑을 통해 올바른 controller를 찾아준다.
 <br>  
   3.  DispatcherServlet은 컨트롤러의 비즈니스 로직을 handlerAdapter에 전달한다.
 <br> 
   4.  컨트롤러에서 비즈니스 로직을 처리한후 Model에 값을 담아서 HandlerAdapter에 전달한다
 <br>  
   5.  DispatcherServlet은 ViewResolver를통해 view에 대한 정보를 통해 실제 view를 찾아준다
 <br>  

   6.  View는 전달받은 Model 데이터를 렌더링해서 DispatcherServlet에서 응답받은 값을 클라이언트에게 보여진다.  
