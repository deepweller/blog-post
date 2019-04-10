# java spring FTP 패시브 모드

*요약*
ftp 접속 문제가 발생하여 원인을 찾던 중, active 모드와 passive 모드의 차이때문에 발생한 문제였다.
액티브모드는 서버 접속 시 
파일질라는 기본으로 패시브모드로 접근하기 때문에 문제가 발생하지 않았던 것.
또한 로컬개발환경에서는 왜 발생하지 않았는지 찾아본 결과 맥의 환경설정 > 네트워크 > 사용중인네트워크의 고급설정에 ftp 설정이 있었다.
여기에 패시브모드로 사용이 디폴트였음.

*이슈*
ftp gui 툴이나 local 환경에서는 ftp 폴더에 접속해서 파일까지 읽는데 개발환경에서는 파일을 가져오지 못하던 이슈가 발생했음

*추가정보*
현재 ftp 접속 라이브러리는 `commons-net` 을 사용하고 있음
```
<dependency>
    <groupId>commons-net</groupId>
    <artifactId>commons-net</artifactId>
    <version>3.6</version>
</dependency>
```

*해결*
ftp 서버 로그인 전 접근방식을 passive 모드로 설정함
```java
client.setSoTimeout(TIMEOUT);
client.enterLocalPassiveMode();
boolean isLogin = client.login(ftpUser, ftpPwd);
```

*참고메일*
> 안녕하세요
> 
> FTP 연결 이슈에 대해서사용하시는 클라이언트가 어떤건지, oooo 측의 방화벽, 네트워크 정책이 어떠한지를 저희가 알고있어야 정확한 답변이 가능할 것 같습니다.
> 그리고 문의해주신 액티브/패시브 모드에 대해서도 사용하시는 클라이언트가 어떤건지가 중요하며,
> 로컬에서 테스트 시 파일질라 같은 클라이언트를 이용 하셨다면 기본 옵션이 패시브로 되어있어서 문제가 발생하지 않았을 것 입니다.
> 
> 또한 액티브 방식의 연결에 실패하신 원인으로 짐작가는 부분은
> 액티브 연결 시서버가 클라이언트에 접속할 포트를 접근가능하도록 열어놓는 작업이 선행되어야 하는데,
> 네트워크/방화벽 정책 등에 의해 막혀있다면 접속이 안될것입니다.
> 
> 액티브 방식의 문제(서버가 클라이언트 접근하는 문제)를 해결하고자 나온 방식이 패시브 방식이며,
> 저희쪽 포트만 정상적으로 열려있으면 접근이 가능 하셨을 것입니다.
> 아래 URL 확인 하시면 이해에 도움이 되실겁니다.
> [https://blog.naver.com/itperson/220774933344](https://blog.naver.com/itperson/220774933344)
> 감사합니다.

### 참고링크
[참고1](https://extrememanual.net/3504)
[참고2](https://extrememanual.net/3504)
[맥 ftp 옵션](https://discussions.apple.com/thread/2203238)
[Java Spring FTP 파일 다운로드 FTP file download :: 알짜배기 프로그래머](https://aljjabaegi.tistory.com/376)
[JAVA 임시 파일 생성, FTP 파일 업로드 :: 알짜배기 프로그래머](https://aljjabaegi.tistory.com/366)
[개발자노트 :: java 에서 ftp 접속 파일 다운로드, 업로드, 삭제 코드](https://jinmanp.tistory.com/entry/java-%EC%97%90%EC%84%9C-ftp-%EC%A0%91%EC%86%8D-%ED%8C%8C%EC%9D%BC-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EC%97%85%EB%A1%9C%EB%93%9C-%EC%82%AD%EC%A0%9C-%EC%BD%94%EB%93%9C)
[Spring-Integration Spring을 사용하여 FTP 파일전송 :: OKIHOUSE](https://okihouse.tistory.com/entry/SpringIntegration-Spring%EC%9D%84-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-FTP-%ED%8C%8C%EC%9D%BC%EC%A0%84%EC%86%A1)
