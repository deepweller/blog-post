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