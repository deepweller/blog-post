### intellij 단축키
다른 분이 정리해 놓은 [포스팅](http://yuns-helloworld.tistory.com/entry/intellij-%EC%9D%B8%ED%85%94%EB%A6%AC%EC%A0%9C%EC%9D%B4-%EB%8B%A8%EC%B6%95%ED%82%A4-%EB%AA%A8%EC%9D%8C)을 보고 하나씩 해보면서 정리

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

alt + insert : 코드 생성(getter, setter, constructors, override method(equals, toString...))  
alt + q : context info (현재 커서가 속한 클래스 정보)  
alt + enter : intention action and quick fixes  

tab, shift + tab : 들여쓰기, 내어쓰기  

shift + enter : 라인 생성 후 새로운 라인 첫줄로 이동(ctrl + shift + enter 와 유사)  

##### not work
ctrl + f1 : show descriptions of error or warning at caret  
ctrl + alt + t : 코드 감싸기(if else, try catch, for, synchronized...) > 터미널 켜짐  
ctrl + alt + l : 코드 정렬(이클립스의 ctrl + shift + f) > 컴퓨터 잠금  
ctrl + enter : 커서를 여러줄로 나 (ctrl + shift + j 반대)  
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

##### not work
ctrl + shift + m : replace structurally

### Usage Search
ctrl + f7 : find usage in file  
ctrl + shift + f7 : highlight usage in file  

##### not work
alt + f7 : find usage > os 단축키 있음, 마우스 우클릭 해서 선택해야함  
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


### 참고
http://yuns-helloworld.tistory.com/entry/intellij-%EC%9D%B8%ED%85%94%EB%A6%AC%EC%A0%9C%EC%9D%B4-%EB%8B%A8%EC%B6%95%ED%82%A4-%EB%AA%A8%EC%9D%8C
