# Spring MVC

## Controller

### Response Type

* 보통 스프링 web mvc 컨트롤러에서 리소스 페이지를 넒길때는 문자열을 리턴하면 `ViewResolver`에 의해 템플릿 엔진이나 서버에서 생성된 static 파일들(thymeleafe, jsp 등)을 찾아서 넘겨주고, json이나 xml 데이터는 리턴 타입에 맞게 response body에 넣어서 응답한다.
* 근데 응답으로 static 파일 자체가 아닌 직접 html 문서 문자열을 전달해야하는 경우가 생겼다.
* 답은 응답형식을 지정하고 응답바디에 넣어서 전달하면 된다.
* `@Controller`에 `@ResponseBody`로 전달해도 동일

```java
@RestController("/v1")
public class SampleController {
    @GetMapping(produces = MediaType.TEXT_HTML_VALUE)
    public String getHtml(HttpServletResponse response) {
        response.setContentType("text/html");
        response.setCharacterEncoding("UTF-8");
        return "<!DOCTYPE html>" +
                "<html lang=\"kr\">" +
                "<head>" +
                "    <meta charset=\"UTF-8\">" +
                "    <title>title</title>" +
                "</head>" +
                "<body>" +
                "hi" +
                "</body>" +
                "</html>";
    }
}
```

#### 참고

* https://stackoverflow.com/questions/17611896/spring-mvc-controller-return-html
