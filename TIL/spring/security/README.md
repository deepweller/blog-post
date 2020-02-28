# Spring Security

스프링 프레임워크에서 지원하는 인증,보안 관련 라이브러리

* 간단하게라고 쓰여있지만 간단하지 않게 로그인, 로그아웃 관련 기능구현 가능
* 간단하게라고 쓰여있지만 간단하지 않게 인증, 권한 관련 기능구현 가능

## Configuration

다양한 시큐리티 설정을 메서드 체이닝 형태로 할 수 있다.

- url 별로 접근 권한 부여 가능
- 로그인, 로그아웃 설정 가능
- resource 파일 인증 제외 가능
- 기타 등등

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/resources/**", "/*.ico");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .antMatchers("/admin/**").access("hasRole('ADMIN')")
                .antMatchers("/user/**").access("hasAnyRole('ADMIN', 'USER')")
                .anyRequest().permitAll()
                .and()

                .formLogin()
                .loginPage("/login")
                .defaultSuccessUrl("/")
                .and()

                .logout()
                .logoutSuccessUrl("/");
    }
}
```

## Authentication token

extends `AbstractAuthenticationToken`

### 구성요소

#### Principal

인증 주체이다. 이 주체를 요구사항에 맞게 정하면 된다. ex)jwt, cookie 등

#### Authorities

해당 인증 주체에 대한 권한이다. `ROLE_ADMIN`, `ROLE_USER` 등 원하는 권한 부여 후 권한에 따라 접근을 제어할 수 있다.

## Filter chain

스프링시큐리티는 여러 내장 필터를 차례대로 거치면서 인증을 한다. 여러 내장 필터 사이에 직접 만든 필터를 추가하여 추가적인 인증을 할 수 있다.

* 필터체인은 이 링크 참조
* https://siyoon210.tistory.com/32

### 주의사항

* 리소스 파일은 api와 달리 대부분 인증이 필요하지 않기 때문에 보통 컨피그 파일에 ignore로 등록해둔다.
* 필터에서 `resource`(css, js 등) 파일들이 걸러지지 않았다.
* 원인은 필터를 빈으로 등록했기 때문이었다.
* 이유는 빈으로 등록된 필터는 스프링이 `application context`에 등록을 한다. 그러면 시큐리티 내에서 필터가 호출되는 것 이외에도 모든 `dispatcher servlet`을 거치는 요청에도 필터가 호출되게 된다.
* 따라서 빈으로 등록하지 말고, 시큐리티 컨피그에만 일반 객체로 등록을 해줘야 한다.
  * 필요한 빈들은 외부에서 주입할 수 있도록 한다.
  * 이거때매 개고생함

```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/resources/**", "/static/**", "/front/**", "/*.ico");
    }

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
          .addFilterBefore(new CustomAuthFilter(securityService, authService), BasicAuthenticationFilter.class);
    }
}
```

**참고**

* https://stackoverflow.com/questions/39152803/spring-websecurity-ignoring-doesnt-ignore-custom-filter
* https://stackoverflow.com/questions/46595911/spring-custom-filter-is-always-invoked
