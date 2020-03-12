# Javscript

## 목록

* [서버에서 받은 데이터로 새탭열기](#새탭열기)

### 새탭열기

#### 이슈

* 서버에서 받은 데이터를 새탭에서 열어서 보여줘야 했다.
* get 방식 api 라면 간단하게 새로운 탭을 열 수 있다. > `window.open()`
* 그런데 쿼리스트링으로 전달할 파라미터의 양이 너무 많아서 post로 request body에 넣어서 보내야 했다.
* 이 경우에는 바로 탭에 url을 넘겨서 열 수 없고, 서버에서 받은 데이터를 기반으로 새로 탭을 열어서 거기에 서버에서 받은 결과 데이터를 보여줘야 한다.
* form을 hidden 처리해서 보여주는 방법도 있지만 이 경우도 파라미터가 너무 많은경우는 모든 데이터를 hidden으로 처리할 수 없다.
* 그래서 서버에서 받은 데이터를 새로운 탭에 전달하는 방식을 사용했다.

#### 방법

* 서버 api가 GET 인 경우

```javascript
let win = window.open(`${baseUrl}/user`, '_blank');
win.focus();
```

* 서버 api가 POST 인 경우

```javascript
return serverRequest({
  url: `${baseUrl}/user`,
  method: 'post',
  data
}).then((response) => {
  console.log(response)
  let wnd = window.open("about:blank", "", "_blank");
  wnd.document.write(response);
});
```
