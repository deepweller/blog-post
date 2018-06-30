### 시작하며
이클립스와는 조금 다른 intellij 단축키와 용어에 익숙해지기 위해서 직접 하나씩 해보고 정리해야겠다. 바로 다 외우긴 힘들어도 내가 써놓은 거니까 찾기 쉽겠지.
다른 분이 정리해 놓은 [포스팅](http://yuns-helloworld.tistory.com/entry/intellij-%EC%9D%B8%ED%85%94%EB%A6%AC%EC%A0%9C%EC%9D%B4-%EB%8B%A8%EC%B6%95%ED%82%A4-%EB%AA%A8%EC%9D%8C)을 보고 하나씩 해보면서 정리해보자.

### Editing
ctrl + space : basic code complition(the name of any class, method or variable)  
ctrl + shift + space : smart code completion(filters the list of methodsand variables by expected type)  
ctrl + shift + enter : complete statement  
ctrl + p : 함수 파라미터 정보 확인(함수 작성 시)  
ctrl + shift + p : 함수 리턴 정보 확인  
ctrl + q : 문서 (javadoc, comment)  
ctrl + mouse : 선택한 항목 detail  
ctrl + o : 메서드 오버라이드 구현  
ctrl + i : 인터페이스 메서드 구현  
ctrl + / : line comment  
ctrl + shift + / : block comment  
ctrl + w : 가장 안쪽 괄호부터 선택  
ctrl + shift + w : ctrl + w 반대  
ctrl + alt + o : import 정리  
ctrl + alt + i : 줄 단위 코드 정렬  
ctrl + x,v / shift + delete,insert : 잘라내기, 붙여넣기(블럭 선택 없으면 라인단위로 수행)  
ctrl + c / ctrl + insert : 복사  
ctrl + shift + v : 목록에서 선택하여 붙여넣기  
ctrl + d : 선택한 블록 또는 라인 복사  
ctrl + y : 커서라인 삭제  
ctrl + shift + j : 선택한 코드를 한 줄로 합침  
ctrl + shift + u : 커서가 있는 곳이나 블럭을 대문자,소문자로 변환  
ctrl + shift + [/] : 가장 가까운 괄호 시작, 종료까지 블럭지정  
ctrl + delete : 커서 다음부터 단어 단위 삭제  
ctrl + backspace : 커서 이전으로 단어 단위 삭제  
ctrl + +/- : 커서가 위치한 곳의 코드블럭을 접었다 펴기 (expand,collapse code block)  
ctrl + shift + +/- : 전체 코드블럭을 접었다 펴기  
ctrl + f4 : 현재 에디터 탭 끄기  
ctrl + alt + t : 코드 감싸기(if else, try catch, for, synchronized...)  
ctrl + alt + l : 코드 정렬(이클립스의 ctrl + shift + f)  
  
alt + insert : 코드 생성(getter, setter, constructors, override method(equals, toString...))  
alt + q : context info (현재 커서가 속한 클래스 정보)  
alt + enter : intention action and quick fixes  
  
tab, shift + tab : 들여쓰기, 내어쓰기  
  
shift + enter : 라인 생성 후 새로운 라인 첫줄로 이동(ctrl + shift + enter 와 유사)  

##### not working
ctrl + f1 : show descriptions of error or warning at caret  
ctrl + enter : 커서를 여러줄로 나누기 (ctrl + shift + j 반대)  
shift + f1 : 코드에 대한 문서를 인터넷 브라우저로 검색  

### Action
shift + shift : search everywhere  
ctri + f : find  
f3 : find next  
shift + f3 : find previous  
ctrl + r : replace  
ctrl + shift + f : find in path (이클립스에서 프로젝트(패키지) 내부 단어 검색기능)  
ctrl + shift + r : replace in path  
ctrl + shift + s : search structurally / sbt shell  

##### not working
ctrl + shift + m : replace structurally  

### Usage Search
ctrl + f7 : find usage in file  
ctrl + shift + f7 : highlight usage in file  
alt + f7 : find usage  

##### not working
ctrl + alt + f7 : show usage  

### Compile and Run
ctrl + f9 : make project (compile modified and dependent)  
ctrl + shift + f9 : compile selected file, package or module  
alt + shift + f10 : select config and run  
alt + shift + f9 : select config and debug  
shift + f10 : run  
shift + f9 : debug  
ctrl + shift + f10 : run context config from editor (현재 에디터의 config를 찾아서 run)  

### Debugging
f8 : step over, 라인 넘어가기  
f7 : step into, 해당 메서드로 들어가기, 메서드 아니면 라인 넘어가기  
shift + f7 : smart step into  
shift + f8 : step out  
f9 : resume  
ctrl + f8 : 커서 라인에 breakpoint 생성  
ctrl + shift + f8 : show breakpoint list  
alt + f9 : run cursor > os 창 최소화  
alt + f8 : evaluate expression > os  

### Navigation
ctrl + n : 클래스검색  
ctrl + shift + n : file 검색  
ctrl + alt + shift + n : symbol 검색  
ctrl + g : go to line  
ctrl + e : 최근 파일 파업  
ctrl + alt + left/right : 최근 커서로 이동  
ctrl + shift + backspace : 최근 수정한 위치로 커서 이동    
ctrl + b / ctrl + click : go to type declaration (이클립스 f3)  
ctrl + alt + b : go to implementation (인터페이스 구현체로 가기)  
ctrl + shift + i : open quick definition popup (해당 메서드, 클래스, 인터페이스 등 선언부분 팝업)  
ctrl + u : go to super-method / super-class    
ctrl + [/] : move to code block end/start  
ctrl + f12 : 파일의 구조(메서드,프로퍼티 등) 팝업  
ctrl + h : type hierarchy  
ctrl + shift + h : method hierarchy   
ctrl + alt + h : call hierarchy   
ctrl + enter / f4 : hierarchy 에서 해당 코드라인으로 가기, 커서까지 가기  
ctrl + f11 : bookmark with mnemonic  
ctrl + #[0-9] : go to numbered bookmark  
  
alt + right/left : 에디터 tab 이동  
  
shift + esc : hide active or last active window(최근 활성화한 window close)  
shift + f2 / f2 : next/previous hightlighted error  
shift + f11 : show bookmark  
    
esc : 에디터로 커서 이동  
  
##### not working
f12 : go back to previous tool window  

##### need to test
ctrl + shift + f4 : close active run/messages/find/... tab  
alt + f1 : select in popup (window 선택 팝업) / select current file or symbol in any view  
ctrl + shift + b : go to type declaration   
alt + up/down : go to previous/next method   
alt + home : show navigation bar  
f11 : toggle bookmark  

### Refactoring
f5 : copy somethings  
f6 : move somethings  
shift + f6 : rename  
ctrl + f6 : change signature (항목 선언부 수정)  
ctrl + alt + n : inline  
ctrl + alt + m : extract method  
ctrl + alt + v : extract variable  
ctrl + alt + f : extract field  
ctrl + alt + c : extract constant  
ctrl + alt + p : extract parameter  

##### need to test
alt  + delete : safe delete  

### VSC/Local history
ctrl + k : commit project to vcs  
ctrl + t : update project from vcs  
alt + shift + c : view recent changes

##### need to test
alt + ` : vsc popup  

### Live templates
ctrl + alt + j : surround with live template (템플릿 선택창)  
ctrl + j : insert live template (템플릿 선택창)  
  
iter : iteration according to jaca sdk 1.5 style  
inst : check object type (instanceof, downcast)  
itco : iterate elements of java.util.Collection  
itit : iterate elements of java.util.Iterator  
itli : iterate elements of java.util.List  
psf + i,s : public static final (int, String)  
thr : throw new  

### General
alt + #[0-9] : tool window 열고 닫기  
ctrl + s : sava  
alt + shift : add to favorites  
alt + shift + i : inspect current file with current profile  
ctrl + ` : switch current scheme  
ctrl + alt + s : 설정    
ctrl + shift + a : find action  
ctrl + tab : tab switch popup (tool window, editor  )

##### need to test
ctrl + alt + y : synchronize    
ctrl + shift + f12 : toggle maximizing editor  
ctrl + alt + shift + s : open project structure dialog  

### 마치며
너무많다... 일단은 무슨 단축키가 있었다는 것만이라도 기억해놓고 찾아서 써야지. 그리고 os단축키랑 겹치는 것도 있어서 자주 쓴다 싶으면 바꿔줘야겠다.

### 참고
http://yuns-helloworld.tistory.com/entry/intellij-%EC%9D%B8%ED%85%94%EB%A6%AC%EC%A0%9C%EC%9D%B4-%EB%8B%A8%EC%B6%95%ED%82%A4-%EB%AA%A8%EC%9D%8C
