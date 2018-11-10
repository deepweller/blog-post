### 시작하며
지난 번에 외부 API 연동을 하면서 인증을 하는 로직을 AOP로 분리했었다. 이번에는 해당 API를 호출하고 로그를 남기기 위해 AOP 사용한 내용을 공유해본다.  
간단하게 상황을 설명하면 외부에서 내가 개발한 API를 호출한다.(호출할 때 내가 허용한 곳에서 호출을 하는지 검증하는 내용을 전 ([포스팅](http://deepweller.tistory.com/29))에서 했다.) 호출을 한 후 상태가 요청한 파라미터와 내가 응답한 파라미터의 로그를 기록한다.


### @After
처음으로는 별 생각없이 `@After`를 사용했다.
```java
@After(value = "execution(* com.test.controller.TestController.*(..))")
public void writeLog(JoinPoint joinPoint) throws RuntimeException {
    //logging
    //joinPoint.getArgs() 를 사용하여 해당 메서드의 파라미터를 가져올 수 있다.
}
```
그런데 `@After`를 사용했을 때 두 가지 문제점이 있었다.  
첫 번째는 요청파라미터는 `joinPoint`를 통해서 가져올 수 있었지만 내가 응답한 리턴파라미터를 가져오기가 애매했다.  
두 번째는 컨트롤러를 통한 서비스에서 예외가 발생했을 때 로그를 정확히 기록하지 못했다.  
그래서 정상처리됐을 때와 예외가 발생했을 때를 구분할 수 있는 방법을 사용했다.

### @AfterReturning, @AfterThrowing
`@AfterReturning`, `@AfterThrowing` 이 두 개의 어노테이션을 사용하면 `@After` 어노테이션을 사용했을 때 보다 더 디테일하게 AOP를 구현할 수 있다. 정상적으로 로직이 수행되어 리턴값이 있을 때와 중간에 예외가 발생했을 때 각각 처리가 가능하다. `returning = "returnValue"` 와 `throwing = "exception"` 를 통해 각각 리턴,예외 객체를 가져와서 사용할 수 있다.  
```java
@AfterReturning(value = "execution(* com.test.controller.TestController.*(..))", returning = "returnValue")
public void setInterfaceLog(JoinPoint joinPoint, ResponseObject returnValue) throws RuntimeException {
    //logging
    //returnValue 는 해당 메서드의 리턴객체를 그대로 가져올 수 있다.
}

@AfterThrowing(value = "execution(* com.test.controller.TestController.*(..))", throwing = "exception")
public void setInterfaceLogException(JoinPoint joinPoint, Exception exception) throws RuntimeException {
    //logging
    //exception 으로 해당 메서드에서 발생한 예외를 가져올 수 있다.
}
```

### 마치며
지난번 글에서도 작성했지만 단점은 내가 작성한 내용을 남들이 알아차리기 힘들다. 로깅이나 인증정도의 로직이 있는지 모른다고 크게 크리티컬한 문제가 발생하지는 않지만 내가 아닌 다른 사람이 이 소스를 유지보수하게 될 때를 대비해서 주석에 Aspect가 존재함을 확실히 기록해야 겠다. (너무 남발하지도 않아야 겠다.)

### 참고
http://snoopy81.tistory.com/291  
https://howtodoinjava.com/spring-aop/aspectj-afterthrowing-annotation-example  