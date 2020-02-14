# CSS

## 목록

* [Flex](#flex)
* [VH, VW](#vh-vw)

### Flex

컨테이너와 그 안에 아이템으로 나뉘며, 컨테이너 내부의 아이템을 쉽게 배치할 수 있는 기능 제공

> `display: flex;`

#### flex-direction

아이템을 배치하는 주축(main-axis)를 결정

* `row`, `row-reverse`
* `column`, `column-reverse`

#### flex-grow

아이템이 컨테이너에서 차지할 너비 비율을 결정

> `flex: 1;` : 25%
> `flex: 2;` : 50%
> `flex: 1;` : 25%

#### Sample

footer 하단 고정 시 flex를 사용

* 푸터와 컨텐츠 전체를 감싸고 있는 부분을 `flex`, `column`로 설정
* content의 `flex-grow`를 1로 설정하여, 나머지영역(푸터)를 가장 하단으로 밀어냄
* 컨텐츠 영역이 화면 높이보다 적은 경우가 있을 수 있으므로 `min-height`를 100vh로 설정

```html
<div class="wrapper">
  <div class="content">
    content
  </div>
  <footer>footer</footer>
</div>
```

```css
.wrapper {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

.content {
  flex: 1;
}
```

#### 참고

* https://heropy.blog/2018/11/24/css-flexible-box/

### VH VW

viewport의 높이값과 너비값의 비율 단위 (1/100)

* 높이가 1000px인 화면에서
  * 100vh == 최대값
  * 1vh == 10px
* 너비도 마찬가지

#### 참고

* https://webclub.tistory.com/356