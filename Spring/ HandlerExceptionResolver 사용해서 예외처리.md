> ### 개요
* 예외가 발생한 후 Servlet을 통해 예외가 전달되면 HTTP Status는 500으로 처리된다. 
발생하는 예외에 따라 응답값을 다른 HTTP Status로 보내줘야 명확히 어떤 예외가 터졌는지 알 수 있다. 
`HandlerException Resolver`를 사용하면 오류 메시지, 형식등을 API마다 다르게 처리할 수 있고 customize가 가능하다. 

* Exception Resolver 동작과정
  * `적용 전`: Request -> Dispatcher Servlet -> Handler Adaptor ->  Dispatcher Servlet ->  response
  * `적용 후`: Request -> Dispatcher Servlet -> Handler Adaptor ->  Exception Resolver -> response

```
@Slf4j
public class MyHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {

      try {
          if (ex instanceof IllegalArgumentException) {
              log.info("IllegalArgumentException resolver to 400");
              response.sendError(HttpServletResponse.SC_BAD_REQUEST, ex.getMessage());
              return new ModelAndView();
          }
          return new ModelAndView();
      }catch (IOException e){
          log.error("resolver ex", e);
      }
      return  null;
    }
}
```
```
@Configuration
public class WebConfig implements WebMvcConfigurer {
   @Override
    public void extendHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        resolvers.add(new MyHandlerExceptionResolver());
    }
}
```
*  `HandlerExceptionResolver` 를 implements 해준후 `resolveException()` 을 override해야한다. request,response,handler,exception 이 넘어온다 넘어온 exception 으로 처리하는 로직을 구현하면된다. 주의할 점은 WevMvcConfigurer 를 상속받은 클래스에 `resolvers.add(new MyHandlerExceptionResolver());` 처럼 등록을 해줘야 handler Exception이 작동한다. 만약 처리할 수 있는 `exceptionResolver`가 없으면 Exption Resolver가 적용안된것 처럼 Dispatcher Servlet에 예외를 전달해준다.(Http Status 500 으로 리턴이된다)

