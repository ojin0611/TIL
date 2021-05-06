# 예외 처리

Spring으로 Application을 제작하다보면 예외 처리가 필요한 코드들이 자주 발생한다. 이 때 모든 경우를 try-catch로 잡으려고 하면 코드가 지저분해진다. 같은 에러를 하나의 코드로 처리할 수 있도록 (AOP 기법) 해주는 방법을 소개하려고 한다.





## ExceptionHandler

`@ExceptionHandler` annotation을 적용해 인자로 catch하고싶은 Exception을 등록했을 때, 그 Exception을 처리해줄 메소드가 된다.

```java
@ExceptionHandler({ Exception1.class, Exception2.class})
public Object nullex(Exception e) { 
    System.err.println(e.getClass()); 
    return "myService"; 
}
```

### 주의사항

- `@Controller`, `@RestController`에만 적용가능하다. 
- return type은 자유롭게 해도 된다.
- `@ExceptionHandler`를 등록한 Controller에만 적용된다. 즉, 다른 Controller에서 Exception이 발생해도 예외를 처리할 수 없다.





## ControllerAdvice

`@ControllerAdvice` 또는 `@RestControllerAdvice` annotation을 사용하면 모든 Controller에서 발생하는 예외를 잡아 처리할 수 있는 클래스가 된다. 아래의 ExceptionControllerAdvice는 `@ControllerAdvice` annotation을 적용한 클래스다.

```java
@ControllerAdvice
public class ExceptionControllerAdvice {

	private Logger logger = LoggerFactory.getLogger(ExceptionControllerAdvice.class);
	
	@ExceptionHandler(Exception.class)
	public String handleException(Exception ex, Model model) {
		ex.printStackTrace();
		logger.error("Exception 발생 : {}", ex.getMessage());
		model.addAttribute("msg", "처리중 에러 발생!!!");
		return "error/error";
	}
	
	@ExceptionHandler(NoHandlerFoundException.class)
	@ResponseStatus(value = HttpStatus.NOT_FOUND)
	public String handle404(NoHandlerFoundException ex, Model model) {
		logger.error("404 발생 : {}", "404 page not found");
		model.addAttribute("msg", "페이지를 찾을 수 없습니다!!!");
		return "error/e404";
	}
	
}

```

위의 `@ControllerAdvice` + `@ExceptionHandler` 조합으로 모든 컨트롤러의 Exception을 캐치할 수 있다. 

참고 : [링크](https://jeong-pro.tistory.com/195)