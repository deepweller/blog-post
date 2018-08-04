### 시작하며
Vue.js 를 진행하고 있는 프로젝트의 프론트엔드 프레임워크로 사용하고 있다. 익숙하진 않지만 샘플과 자료들을 찾아가며 진행하고 있다.  
이번에는 특정 조건에 따라 class 속성에 값을 추가하는 방법을 적용해봤다. 예를들어 특정 상태값에 따라 버튼의 css를 다르게 적용해야할 때 사용할 수 있다.

### 샘플코드
```html

<div class="hide_btn on">

<div class="hide_btn">

```
위 샘플을 보면 조건에 따라 클래스에 on 값이 추가되거나 없어지거나 해야한다. vue는 클래스 바인딩을 `v-bind:class`로 할 수 있다. 참고로 v-bind는 생략이 가능하여 `:class`로만 작성이 가능하다.

```html

<div :class="{ on : isHideImage }">

```
위와 같이 작성하면 `isHideImage` 값이 true 면 class속성에 on 값이 추가된다.  
그런데 상태값이 true,false 만 있지 않은 경우도 있고 제일 위의 샘플을 보면 `hide_btn`값은 항상 있어야 한다. 이러한 경우에 단순 flag 변수가 아닌 함수의 return 값에 따라서 class 속성의 값을 추가할 수 있다.
```html

<div class="hide_btn" :class="{ 'on' : isStateTwo(video.state) }">

```
```javascript
isStateTwo: function (state) {
                if(state == 2) return true;
                else false;
            }
```
위와 같이 class에는 `hide_btn`가 기본으로 있으며 isStateTwo() 함수의 리턴값에 따라 `on`을 추가할 수 있다.

### 마치며
아직 vue.js에 익숙하지 않고 모르는 부분이 많아 다른 더 좋은 방법이 있을지도 모른다. 일단 여러가지 케이스를 경험해보다보면 어떻게 vue 동작과 랜더링 방식에 익숙해 질 것 같다.

### 참고
https://kr.vuejs.org/v2/guide/class-and-style.html  
https://laracasts.com/discuss/channels/vue/vuejs-add-class-if-specific-property-is-set-to-a-specific-value  
