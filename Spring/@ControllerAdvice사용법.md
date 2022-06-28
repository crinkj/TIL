> ### 개요
* `@ExceptionHandler` 를 사용하면 예외처리는 가능하지만 해당 어노테이션이 달려있는 컨트롤러 내에서만 동작이 가능하므로 확장적이지 않다. `@ControllerAdvice` 을 통해 전역적으로 또는 설정해서 원하는 패키지까지 예외처리를 핸들링 할 수 있다. `@RestControllerAdvice`는 `@ResponseBody` + `@Controller` 와 같은 기능이다.

```
@RestControllerAdvice("web.exception")
public class ControllerAdvice {

     @ExceptionHandler
    public ResponseEntity<ErrorResult> illegalExHandlerWithResponseEntity(IllegalArgumentException e) {
        log.error("[exceptionHandler] ex", e);
        ErrorResult errorResult = new ErrorResult("error", e.getMessage());
        return new ResponseEntity(errorResult, HttpStatus.BAD_REQUEST);
    }

    @ExceptionHandler
    public ResponseEntity<ErrorResult> exceptionHandler(Exception e) {
        ErrorResult errorResult = new ErrorResult("error", "내부 오류");
        return new ResponseEntity(errorResult, HttpStatus.INTERNAL_SERVER_ERROR);
    }
```
* 위와같이 `@RestControllerAdvice()`안에 패키지를 지정할 수 있다. 패키지를 지정안하면 glboal(전역)으로 적용이된다.

> ### 결론
* 전역적으로 예외 처리를 해야하는경우 `@ExceptionHandler`를 사용해주고 `@ControllerAdvice`를 사용해 적용해준다. 또한 특정 패키지에만 사용하고싶을경우에는 `@ControllerAdvice()` 안에 패키지 경로를 지정해준다.   

> ### 참조
* 김영한님의 인프런강의[스프링 MVC 패턴 2]

