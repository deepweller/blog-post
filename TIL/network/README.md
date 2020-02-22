# Network

## 목록

* [HTTP](#HTTP)
* [SSO](#SSO)

### HTTP

HTTP 통신관련

#### Transfer-Encoding: Chunked

* 일반적으로 http로 데이터를 주고받을 때, response body에 데이터를 담아서 전달한다.
* 보통은 `Content-Type` 과 `Content-Length` 를 헤더에 담아 데이터의 타입과 길이를 알려준다.
* 그런데 바디에 담긴 데이터가 큰 경우 통신하는 과정에서 클라이언트가 대기하는 시간이 길어질 수 있다.
* 이 때, 컨텐츠 길이 대신 `Transfer-Encoding: Chunked`를 헤더에 넣고 바디에는 읽어드릴 데이터 길이와 데이터를 끊어서 전달한다.
* 바디 마지막에 0이 있을때 까지 데이터를 끊어서 전달하게 되고, 클라이언트는 한번에 데이터를 모두 받을 때까지 기다리지 않게 된다.

**이슈**

* L7 로드밸런서를 만들어보는 중 text 타입은 소켓으로 잘 전달이 되는데, 유독 json과 html 타입은 정상적으로 응답이 전달이 안됐다.
* 차이를 보던중 헤더에 위 내용과 같은 차이가 있음을 발견하고, chunked 대신 길이를 넣어주었더니 정상적으로 전달이 됐다.
* 다만, http clinet에서 청크 바디를 그대로 전달할 수 있는 방법은 찾아봐야 할 것 같다.

**샘플**

```http
HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Sun, 16 Feb 2020 01:21:41 GMT
Keep-Alive: timeout=60
Connection: keep-alive

15
"["1","2","3"]"

0
```

```http
HTTP/1.1 200 
Content-Type: text/plain;charset=UTF-8
Content-Length: 5
Date: Sun, 16 Feb 2020 01:21:38 GMT
Keep-Alive: timeout=60
Connection: keep-alive


test/1234
```

**참고**

* https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Transfer-Encoding
* https://b.pungjoo.com/entry/Transfer-Encoding-chunked-VS-Content-Length

### SSO

**SSO(Single Sign On) : 단일 인증으로 여러 애플리케이션에 인증하는 방식**

* 여러 서비스에 여러번 인증하는 불편함을 줄여줌
* 대표적으로 구글 애플리케이션은 모두 최초 구글계정으로 로그인이 되어있으면, 지메일, 구글드라이브 등 추가적인 인증없이 사용이 가능하다.
* 일반 서비스 외에도 사내 시스템에서도 통합인증을 사용할 수 있다.
* SSO를 인증하는 방식은 다양하다.  (`Cookie`, `JWT`, `OAuth` 등..)
  * 별도 에이전트에서 인증
  * 토큰 기반 인증
  * 기타등등

#### 구현 절차

* 서버인증은 기본적으로 `spring security`를 사용하고, 각종 커스텀 인증, 토큰, 필터등을 추가해서 요청을 검증함
  * sso뿐만 아니라 스프링에서 회원, 인증관련 로직은 security 기반으로 구현
* 클라이언트 인증은 `vue-admin-element`에 구현되어있는 `permission`, `user` 등을 이용하여 구현

1. 다른 사이트에서 로그인 하여 인증을 한다.
    1. 다른 사이트에서 인증으로 생긴 쿠키 기반으로 sso를 구현한다.
    1. 먼저 인증된 사이트를 A, sso 방식으로 새로 접근할 사이트를 B라 한다.
    1. A 사이트에서 로그인한 결과로 `AUTH` 라는 쿠키가 생성된다.
1. A 에서 B 의 특정 페이지로 접근한다.
    1. vue-admin-element는 기본적으로 `Admin-Token` 이라는 쿠키값을 사용하여 권한 로직이 구현되어 있다.
    1. A 에서 B 로 접근할 때 쿼리스트링이나 다른 기타 방법들로 `Admin-Token` 쿠키에 들어갈 value를 담아서 전달한다.
    1. vue-admin-element 에서 기본적으로 / 경로에서 리다이렉트 경로를 결정하기 때문에 원하는 리다이렉트 경로도 같이 전달한다.
1. 특정 어드민 페이지로 접근했을 경우 보유한 쿠키로 서버에서 인증작업을 한다.
    1. spring security custom filter chain 에서 `AUTH`가 유효한 쿠키인지 검증한다.
    1. `Admin-Token` value가 적절한 값인지 검증한다.
    1. 두 쿠키가 모두 유효하면 `Admin-Token` 쿠키를 생성해준다.
1. `Admin-Token` 토큰으로 프론트에서 인증작업을 하고, 원하는 페이지로 리다이렉트 시켜준다.

#### vue admin element 에서의 SSO

vue 관련, 특히 vue-admin-element 관련 자료는 중국어가 대부분이다.. 번역기가 필수 !

* /permission.js

```javascript
router.beforeEach(async(to, from, next) => {
  // start progress bar
  NProgress.start()

  // set page title
  document.title = getPageTitle(to.meta.title)

  // determine whether the user has logged in
  const hasToken = getToken()

  // 인증 토큰이 있는 경우
  if (hasToken) {
    // 기존 로직과 동일
  } else {
    // 인증 토큰이 없는 경우
    if (whiteList.indexOf(to.path) !== -1) {
      next()
    } else {
      const param = param2Obj(window.location.href)
      //유효한 토큰
      if (param.token) {
        store.dispatch('user/setSSOUserToken', param.token)
        next(`/login?redirect=${param.path}`)
      } else {
        next(`/login?redirect=${param.path}`)
      }
    }
  }
})
```

* store/modules/user.js

```javascript
setSSOUserToken({ commit }, ssouserToken) {
    commit('SET_TOKEN', ssouserToken )
    setToken(ssouserToken)
  }
```

#### 참고

* https://github.com/PanJiaChen/vue-element-admin/issues/1188
* https://www.cnblogs.com/hwubin5/p/11914958.html
