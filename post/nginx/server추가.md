### 시작하며
Spring boot로 간단한 웹서비스를 구축하는 튜토리얼을 진행 중이다. ([tutorial](http://jojoldu.tistory.com/250?category=635883))  
튜토리얼을 진행하고 있는데, nginx를 리버스 프록시 서버로 활용하여 무중단 배포를 진행하는 과정에서 하나의 프록시서버로 내 서비스와 친구의 서비스를 구분해야 했다. (AWS EC2를 연습할겸 계정을 친구와 공유하여 사용하고 있다.)  
nginx.conf 파일에 location이나 server 블록을 추가하면 될 것 같은데 정확하게 감이 오지 않았다. 위 내용을 해결하기 위해 진행한 내용을 기록한다.

### nginx, 리버스 프록시 서버

![nginx](/Users/jinyeong/Downloads/nginx.png)

일단 nginx, 리버스 프록시 서버(reverse proxy server) 모두 생소했다. nginx는 아파치와 같은 웹서버라는 것만 들어봤고, 프록시는 '웹서버에서 요청을 받아 각 요청을 적절한 서버의 위치로 이동시켜주는 역할이다.' 정도로만 알고 있었다. 각각을 알아봤다.
> **nginx**   
> 웹서버이다. 높은 점유율을 차지하고 있는 아파치의 기술적인 한계를 보안하기 위해 나온 제품이라고 한다. 아파치에 비해 다수의 연결에 효율적이고, 적은 리소스를 사용한다. 

> **리버스 프록시 서버**  
> 서버 앞단에 웹서버를 이용하여 사용자가 바로 서버에 접근하지 않고 앞에 있는 웹서버에 요청을 하도록 하는 역할을 한다. 사용자의 요청에 따라 리버스 프록시 서버(앞단의 웹서버)는 서버의 포트나 디렉토리에 연결해 준다.  
> 이렇게 하면 사용자가 직접하는 요청과 프록시 서버를 거친 요청이 다르기 때문에 직접적으로 노출되지 않는 이점이 있고, 하나의 요청으로 여러 형태로 처리하는 분산처리가 가능하다.

프록시 서버에는 리버스 프록시 외에도 포워드 프록시 서버도 있다. 관련 내용은 [여기](http://brownbears.tistory.com/191)를 참고하자.  
이처럼 nginx를 리버스 프록시 서버로 웹서버의 앞단에서 사용자의 요청을 받게하면, 사용자의 직접적인 서버로의 접근을 막을 수 있다. 속도적인 측면도 향상이 있다고는 하지만 튜토리얼로는 체감하긴 힘들다.

### server block 추가하기
nginx를 설치하면 아래 경로에 설정파일이 존재한다.
> /etc/nginx/nginx.conf

위 파일에서 server 블록을 추가하면 된다. 간단하게 살펴보면
```conf
http {
    ... 생략
    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  localhost;
        root         /usr/share/nginx/html;

        ... 생략
    }

    server {
        listen       8081;
        listen       [::]:8081;
        server_name  localhost;

        include /etc/nginx/default.d/*.conf;

        ... 생략
    }
}
```

위와같이 http블록 하위에 server블록이 있다. http블록은 하위에 server와 location 블록을 가진다. server블록은 [가상호스트](https://opentutorials.org/module/384/4529) 개념이다. (아파치의 VirtualHost 개념)  
한대의 서버로 여러개의 서비스를 운영하기 위한 방법이다. 가상호스트를 사용하면 서버는 한대지만 웹서비스 사용자는 각각 다른 도메인이나 IP, port로 접속하게 된다. 그러면 서버는 각각 가상호스트에 정해져 있는 디렉토리나 서버내부포트로 연결해 준다.  
nginx에서 가상호스트를 사용하기 위해서 listen의 포트넘버나 root의 디렉토리를 수정하면 된다. 위 내용으로 보면 디폴트는 80포트이기 때문에 사용자가 서버에 접속하면 root 디렉토리의 내용을 사용자에게 응답한다. 사용자가 8081포트로 접속하게 되면 ../default.d/*.conf 파일에 있는 내용을 사용자에게 응답하게 된다.

마지막으로 파일수정 후 nginx를 재시작해야한다.
> sudo service nginx reload

### 마치며
백엔드는 공부히면 할수록 끊임없이 많은 영역이 생기는 것 같다. 개발환경에서 운영영역까지 정말 넓은 범위를 알아야 하는 것 같다. 문제가 생기거나 필요한 기능이 있을 때 마다 찾은 내용들을 그때그때 정리해야 겠다. 추가로 웹서버, 컨테이너가 실제로 어떻게 동작하는지도 알아봐야겠다.

### 참고
http://brownbears.tistory.com/191  
https://opentutorials.org/module/384/4526  
http://interconnection.tistory.com/27  
https://m.blog.naver.com/jhc9639/220967352282  