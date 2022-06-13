> ### 개요
* 예외가 발생한 후 Servlet을 통해 예외가 전달되면 HTTP Status는 500으로 처리된다. 
발생하는 예외에 따라 응답값을 다른 HTTP Status로 보내줘야 명확히 어떤 예외가 터졌는지 알 수 있다. 자바에서 지원해주는 예외처리도 있고 스프링에서 지원해주는 예외 처리도 있다. 예외 처리하는 다양한 방법들을 알아보자.

> ### HandlerExceptionResolver 예외처리
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

> ### @ExceptionHandler 사용하기 (실무에서 자주 사용하는 방식)
* `ResponseStatusExceptionResolver`는 예외에 따라서 HTTP 상태 코드를 지정해주는 역할을 한다.

```
@Data
@AllArgsConstructor
public class ErrorResult {
    private String code;
    private String message;
}
```
```
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(IllegalArgumentException.class)
    public ErrorResult illegalExHandler(IllegalArgumentException e) {
        log.error("[exceptionHanlder] ex", e);
        return new ErrorResult("BAD", e.getMessage());
    }

```
* `ErrorResult` 라는 response 클래스를 만들어준후 `@ExceptionHandler`를 달아준후 parameter에 원하는 exception 을 담아준후 `ErrorResult` 안에 message에 `e.getMessage()`함수를 통해 에러메세지를 담아준다. `@ResponseStatus(HttpStatus.BAD_REQUEST)`를 지정해주지 않으면 예외가 터져도 HTTP 상태코드가 200(성공)이 된다. Exception 처리는 @ExceptionHandler가 가지고있는 해당 컨트롤러만 적용이된다.
    
```
    @ExceptionHandler
    public ResponseEntity<ErrorResult> illegalExHandlerWithResponseEntity(IllegalArgumentException e){
        log.error("[exceptionHandler] ex", e);
        ErrorResult errorResult = new ErrorResult("error", e.getMessage());
        return new ResponseEntity(errorResult,HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler
    public ResponseEntity<ErrorResult> exceptionHandler(Exception e){
        ErrorResult errorResult = new ErrorResult("error", "내부 오류");
        return new ResponseEntity(errorResult,HttpStatus.INTERNAL_SERVER_ERROR);
    }
```
* 위와 같이 `ResponseEntity`로 감싸준후 httpStatus code를 지정해주면 `@ResponseStatus()` 를 사용안해도된다. 
```
@ExceptionHandler(부모예외)
public String parent(부모 예외 e)

@ExceptionHandler(자식예외)
public String child(자식 예외 e)
```
* 주의점: Spring은 항상 자식 객체에 우선권을 가진다. 자식 클래스가 예외가 터지면 자식예외도 처리하고 부모 예외도 처리한다. 하지만 부모클래스가 예외가 터지면 부모예외만 처리된다.

> ### 결론
* 예외 처리는 중요하다. 예외가 터지면 정확하게 어떤 에러가 터졌는지 그에 맞게 HttpStatus 상태값과 같이 전달을 해줘야 클라이언트와 서버간의 안정적이게 통신이된다. 

> ### 참조
* 김영한님의 인프런강의[스프링 핵심원리 기본]
  