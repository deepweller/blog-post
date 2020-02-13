# Vuejs

## keep-alive

컴포넌트 전환 시 해당 컴포넌트를 destroy하지 않고, 캐싱하여 해당 컴포넌트를 다시 사용할 때 다시 랜더링을 하지 않고 캐싱된 컴포넌트를 가져옴

### 선언단위

* 컴포넌트
  * 컴포넌트 단위로 캐싱할 수 있다.

```html
<keep-alive>
  <my-component></my-component>
</keep-alive>
```

* router view
  * 라우터뷰 단위로 캐싱할 수 있다. 라우팅되는 컴포넌트(뷰)가 캐싱된다.

```html
<keep-alive>
  <router-view />
</keep-alive>
```

### props

* include, exclude
  * 캐싱될 컴포넌트를 지정하거나 뺄 수 있음.
  * props 내에 들어가는 내용은 컴포넌트의 name
  * `<keep-alive include="a,b">`
  * `<keep-alive :include="['a', 'b']">`
* max
  * 캐싱 될 컴포넌트 max count를 지정할 수 있음.
  * 꽉 찬 경우 오래된 컴포넌트 부터 삭제
  * `<keep-alive :max="10">`

## 참고

* https://vuejs.org/v2/guide/components-dynamic-async.html
* https://vuejs.org/v2/api/#keep-alive
* https://velog.io/@kyusung/VUE-keep-alive