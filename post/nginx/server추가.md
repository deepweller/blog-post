### 시작하며
Spring boot로 간단한 웹서비스를 구축하는 튜토리얼을 진행 중이다. ([tutorial](http://jojoldu.tistory.com/250?category=635883))  
AWS EC2를 연습할겸 계정을 친구와 공유하여 사용하고 있다. 아무튼 튜토리얼을 진행하고 있는데, nginx를 리버스 프록시 서버로 활용하여 무중단 배포를 진행하는 과정에서 하나의 프록시서버로 내 서비스와 친구의 서비스를 구분해야 했다.  
nginx.conf 파일에 location이나 server 블록을 추가하면 될 것 같은데 정확하게 감이 오지 않았다. 위 내용을 해결하기 위해 진행한 내용을 기록한다.

### nginx, 리버스 프록시 서버
일단 nginx, 리버스 프록시 서버(reverse proxy server) 모두 생소했다. nginx는 아파치와 같은 웹서버라는 것만 들어봤고, 프록시는 '웹서버에서 요청을 받아 각 요청을 적절한 서버의 위치로 이동시켜주는 역할이다.' 정도로만 알고 있었다. 각각을 알아봤다.
> nginx   
> 웹서버이다. 높은 점유율을 차지하고 있는 아파치의 기술적인 한계를 보안하기 위해 나온 제품이라고 한다.
> 아파치에 비해 다수의 연결에 효율적이고, 적은 리소스를 사용한다. 

> 리버스 프록시 서버  
> 

프록시 서버에는 리버스 프록시 외에도 포워드 프록시 서버도 있다. 관련 내용은 [여기](http://brownbears.tistory.com/191)를 참고하자.  
이처럼 nginx를 리버스 프록시 서버로 웹서버의 앞단에서 사용자의 요청을 받게하면, 사용자의 직접적인 서버로의 접근을 막을 수 있다. 속도적인 측면도 향상이 있다고는 하지만 튜토리얼로는 체감하긴 힘들다.

### server block 추가하기

### 마치며


### 참고
http://brownbears.tistory.com/191
https://opentutorials.org/module/384/4526
http://interconnection.tistory.com/27
https://m.blog.naver.com/jhc9639/220967352282



버추얼호스트
엔진엑스, 아파치 제대로 비교
엔진엑스 컨프파일에 대해 설명

nginx 에서 server 블록 추가하여
여러 서비스를 포트만 바꿔서 운영하기

@deepweller
deepweller commented 54 minutes ago
안녕하세요. 글 너무 잘보고 있습니다.
궁금한 점이 하나 있는데 혹시 nginx.conf 파일에 두개의 server 블록을 둬서 두개의 서비스를 사용할 수 있을까요?

@jojoldu
jojoldu commented 51 minutes ago
Owner
@deepweller 아 넵 가능합니다.
보통 작은 회사에서는 사내에서 사용하는 작은 규모의 서비스(관리자 서비스등)는 한 서버에 다 올리고 Nginx로 서로 다른 포트를 Proxy pass 하기도 합니다.

@deepweller
deepweller commented 46 minutes ago
답변 빨리 주셔서 감사합니다.
server 블록에 listen 에 있는 포트를 바꿔서 추가하니깐 되네요!
감사합니다!