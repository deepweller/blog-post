# Network

## 목록

* [HTTP](#HTTP)

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

