### 시작하며
`AngularJS`는 `SPA(Single Page Application)` 구조를 가지는 프론트 프레임워크이다. 하나의 페이지로 되어있기 때문에 페이지 사용에 필요한 리소스를 최초에 모두 로드한다. 따라서 초기 구동이 느리지만 네이티브앱과 유사한 UX를사용자에게 제공할 수 있는 장점이 있다.  
또한 하나의 페이지로 되어있기 때문에 각 페이지간 이동을 위해서는 `Routing`이 필요하다.
>라우팅은 페이지의 이동 경로를 정해주는 역할을 하며 여기서는 페이지간 전환을 관리해주는 역할을 한다. 이와 비교되는 방식은 여러개가 있지만 대표적으로 html의 a 태그를 이용한 페이지 이동방식이다.

이와 동시에 위에서 언급한 것 처럼 초기에 모든 리소스를 로딩하다 보니 사용자가 자주 사용하지 않는 리소스까지 모두 로딩되는 단점이 있다.  
이를 해결하기 위해 `lazyload`가 필요하다. 
> lazyload는 리소스를 미리 준비해 놓는 것이 아닌 필요할 때 로딩한다는 개념이다.

AngularJS에서 페이지를 이동할 때 필요한 리소스를 로드하기 위한 기능을 사용하기 위해 다음 라이브러리를 사용할 수 있다.
> routing : [UI-Router](https://ui-router.github.io)  
> lazyload : [ocLazyLoad](https://oclazyload.readme.io)

위 라이브러리를 이용하여 페이지를 전환할 때 필요한 리소스를 로딩하는 기능을 구현해본다.

### ui-router 사용하기
ui-router 공식사이트에 가면 자세한 튜토리얼과 문서들이 있다. 여기서는 간단한 사용법만 알아본다.
```javascript
var myApp = angular.module('MyApp', ['ui.router']);
```
위 코드 처럼 최상위 app 모듈에 `ui.router` 모듈을 추가하여 사용할 수 있다.  
다음으로는 모듈을 추가한 후 myApp모듈의 config에 `$stateProvider`를 주입한 후, `state` 를 등록한다. state에는 url, template(또는 템플릿 파일의 url), controller 등을 정의할 수 있다.
```javascript
myApp.config(function($stateProvider) {

$stateProvider
    .state('hello', {
        url: '/hello',
        template: '<h3>hello world!</h3>'
    }).state('about', {
        url: '/about',
        template: '<h3>Its the UI-Router hello world app!</h3>'
    });

}
```
위 처럼 state를 등록하게 되면 각각 url로의 요청이 들어오게 되면 해당 템플릿을 사용자에게 서버가 제공해준다. 이때 전달되는 템플릿은 `<ui-view></ui-view>` 태그 위치에 삽입된다. 위 태그를 페이징을 처리할 곳에 넣으면 된다.
```html
<body ng-app="myApp">
  <ui-view></ui-view> <!-- 전달되는 템플릿으로 교체될 위치 -->
</body>
```

url을 호출하는 방법은 `service`(angularjs에서 공통으로 사용하는 로직) 등에서 `state.go`를 이용하는 방식과 태그에서 `ui-sref`속성을 이용하는 방식이 있다.  
* state.go 방식
```javascript
//서비스에 $state를 주입해야 한다.
function controller($state){
    //...생략
    
    /**
     * state에 등록한 url을 전달한다.
     * 그러면 해당 url로 이동하기 위한 state를 호출한다.
     */
    $state.go(url); 
    //...생략
}
```
* ui-sref 속성 방식
```html
<!-- state에 등록한 url을 ui-sref 속성에 등록한다. -->
<a ui-sref="url" ui-sref-active="active">hello</a>
```
요약하면  
1. 모듈의 config에 `stateProvider` + `state`를 등록한다.
2. html(template)에 라우팅된 템플릿이 들어갈 위치에 `<ui-view>`태그를 삽입한다.
3. 서비스 또는 템플릿에서 이벤트를 발생시킨다. (`state.go` or `ui-sref`)
4. `state`에 등록한 탬플릿이 `<ui-view>`태그 위치에 삽입되어 페이지가 로딩된다.

### ocLazyload 사용하기
ui-router와 마찬가지로 ocLazyload 공식사이트에 가면 자세한 튜토리얼과 문서들이 있다. 여기서는 간단한 사용법만 본다.
```javascript
var myApp = angular.module("MyApp", ["oc.lazyLoad"]);
```
위 코드 처럼 최상위 app 모듈에 oc.lazyLoad 모듈을 추가하여 사용할 수 있다.  
다음으론 모듈의 컨트롤러에 load할 파일을 등록해야한다. 특정 이벤트가 발생하면 어떠한 파일들을 로드하겠다는 로직이 추가되어야 한다.
```javascript
myApp.controller("MyCtrl", controller)

function controller($ocLazyLoad) {
  this.loadFile = function() {
    $ocLazyLoad.load('testModule.js');
  }
});
```
위 코드와 같이 뷰에서 특정 이벤트가 발생하여 controller에 loadFile이라는 메소드가 호출이 되면 testModule.js 파일이 로드된다.

### ui-router와 ocLazyload 같이 사용하기
여러개의 페이지로 구성된 웹앱일 경우 모든 리소스(js, html 등등)를 처음에 로딩하면 최초 로딩속도에 영향을 미친다. 모든 파일의 크기만큼 서버의 응답을 추가로 기다려야하기 때문이다. 따라서 페이지를 이동할 때 필요한 파일들을 로딩할 수 있다면, 그리고 최초페이지에는 최초페이지를 로딩할 때 필요한 파일만을 가져온다면 위 문제를 크게 개선할 수 있다. 이는 규모가 커질수록 체감되는 성능향상이 크다.

소스는 크게 바뀌는 부분이 없고, 등록한 state에 lazyload할 파일을 등록해 주면된다.
```javascript
$stateProvider
    .state('hello', {
        url: '/hello',
        //template대신 templateUrl로 html파일을 지정할 수 있다.
        templateUrl: 'app/MyTpl.html', 
        controller: 'app/MyCtrl.js',
        resolve: {
            lazyLoad: function ($ocLazyLoad) {
                return $ocLazyLoad.load({
                    //여러파일을 가져오려면 배열에 추가하면 된다.
                    files: ['app/MyCtrl.js'/*,app/추가파일.js*/]
                });
            }
        }
    });
```
위 소스에서 state 내에 resolve를 사용했는데 이는 lazyload라는 promise객체를 생성한다. (lazyload 외에도 사용할 수 있는 다른 객체가 있다.) 이 pormise객체가 resolve 되어야만 라우팅이 완료됨을 의미한다. 즉 파일이 로드되어야 라우팅이 진행된다.  
위와 같이 state를 수정하면 `/hello` url로 사용자의 요청이 들어오면 state에 있는 정보를 토대로 페이지를 이동시킨다.


### 끝으로
대부분의 프론트앤드 프레임워크를 이용하여 만든 페이지는 SPA구조이다. 처음에는 이 개념을 이해하기가 어렵다. 라우팅의 개념도 처음에는 확 와닿지 않는다. 실제로 간단한 튜토리얼을 진행해 보고, 만든 페이지의 URL이 어떻게 변경되는지, 또 개발자 도구에서 source탭에 어떤 js파일이 로딩이 되는지를 직접 확인해보면 감이온다.   

추가로 참고한 페이지에 더 상세한 사용법, 튜토리얼이 포함되어 있다.
### 참고
https://ui-router.github.io  
https://github.com/angular-ui/ui-router/wiki  
https://oclazyload.readme.io  
https://poiemaweb.com/js-spa