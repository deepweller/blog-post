### 시작하며
업무, 커뮤니티 등 slack을 사용할 일이 많다. 워낙 많이들 사용하기 때문에 잘 모르는 사람이 있을지 모르겠지만 처음 접하는 사람은 그냥 메신저와 차이를 못느끼거나, 슬랙의 workspace나 channel에 대해 생소하게 느껴질 수 있다. 간단하게 라도 어떻게 활용이 가능한지, 왜 많이들 사용하는지 알아보자.

### Workspace
workspace는 가장 큰 그룹이다. 작게는 팀이 될 수도 있고, 크게는 회사 단위가 될 수도 있다. workspace를 생성하면 접근이 가능한 url이 나오고 다른 사람들은 이 url로 접속하여 join할 수 있다. 또는 workspace를 생성한 사람 또는 매니저권한을 가진 사람이 초대장으로 해당 url을 전송할 수 있다. 초대장을 받은 사람은 메일로 가입을 할 수 있고, 가입한 메일로 오는 인증을 완료하면 workspace에 참여하게 된다.   
![workspace](workspace.png)

### Channel
channel은 workspace내에 무수히 많은 방이라고 생각하면 된다. workspace에 속해있는 사람은 누구나 channel을 생성하고 다른 channel에 join할 수 있다. (channel 생성은 Channels 옆 + 버튼을 누르거나 Channels를 누른 후 Create Channel을 클릭한다.) 그때그때 필요한 channel을 생성해서 사용할 수 있고, 장기적으로 필요한 channel을 미리 생성해 놓고 구성원이 join할 수 있다.   
![create channel](createchannel1.png)   
![create channel](createchannel2.png)   
channel은 public과 private 두 종류가 있다. public은 누구나 join할 수 있으며 private은 초대를 받은 workspace구성원만 참여할 수 있다. 용도에 맞게 public과 private channel을 적절히 사용하면 된다.
* public channel : # + 채널명
* private channel : 자물쇠+채널명    

![channel](channel.png)   

### Direct Message
다이렉트 메시지는 1:1 대화라고 생각할 수 있다. (물론 여러명을 초대하여 다이렉트 메시지도 가능하다.) channel은 팀 또는 맡은 업무, 정보공유 등과 같은 하나의 목적을 가진 공간이라고 한다면, 다이렉트 메시지는 channel로 만들기에는 일시적이며, 간단한 대화를 하는 용도라고 보면 된다. (사실 둘의 구분이 애매하지만 channel을 세분화하여 대부분의 대화 또는 정보공유를 channel에서 하는 것을 권장한다.) 다이렉트 메시지는 보통 workspace 구성원들과의 간단한 채팅을 목적으로 사용한다.   
![direct message](dm.png)   
다이렉트 메시지도 channel과 동일하게 + 버튼으로 시작할 수 있다. 여러명을 초대하여 동시에 대화도 가능하다.

### slackbot
슬랙봇은 개인용 챗봇이다. 본인이 기능을 추가하여 챗봇과 질의응답을 할 수 있으며, 알림을 받을 수도 있다. 또 workspace의 관리자가 기능을 추가하여 개개인에게 알림 등을 전달할 수 있다. (slackbot은 나와의 Direct message + 챗봇 + 부가기능이다.)   
![slackbot](bot.png)   

### App
slack이 유용한 데에는 많은 App이 있기 때문이다. 크롬의 확장프로그램과 비슷하게 slack에는 여러가지 App을 사용하여 부가기능을 추가할 수 있다. 예를 들어 incomming webhook을 이용하여 소스나 타 사이트에서 직접 workspace로 메시지를 전송할 수 있다. mailclack으로 메일알림을 슬랙으로 받고 다시 또 보낼수도 있다.(무료로 사용하면 한달에 100건이다.) polly로는 간단한 투표를 만들 수도 있고 그 밖에도 구글캘린더, 구글드라이브, todo, github 등등 정말 많은 App이 있다. 필요한 App을 추가해서 흩어져 있는 알림들을 한곳으로 모으면 프로젝트를 좀 더 수월하게 진행할 수 있다. (slack만 있으면 다른 메신저, 메일 등의 사용빈도가 크게 낮아진다.)

### 끝으로
slack은 3개의 과금정책이 있다. 자세한 내용은 공식사이트에서 확인하면 된다. (35명정도되는 팀으로 slack을 메신저겸 이런저런 plugin을 붙여서 사용했는데 풍족하진 않았다.)   
app에도 메시지의 개수가 정해져 있는 경우가 있어서 빌드나 테스트결과 또는 메일연동, webhook 등을 많이 사용하면 금방 한도가 끝났다는 메시지를 볼 수 있다. 또 그만큼 많은 내용을 보내게 되면 지난 메시지들이 삭제되기 때문에 조금만 지나도 지난내용이 검색이 안되는 경우가 있다.   
사용량이 많으면 적절한 정책을 골라서 금액을 지불하면서 사용하거나, 위의 불편함을 감수하면서 사용하면 된다. 하지만 규모가 크지 않은 IT회사 또는 부서에서는 아주 좋은 선택이라고 생각한다.

### 참고
https://slack.com   
https://api.slack.com