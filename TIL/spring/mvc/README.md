# Spring MVC

## Controller

### Response Type

응답으로 static resources 파일들(thyreaf, jsp 등)이 아닌 직접 html 문서를 작성해서 전달해야하는 경우가 생겼다. 응답형식을 지정하고 응답바디에 넣어서 전달하면 된다. (`@Controller`에 `@ResponseBody`로 전달해도 동일)

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

