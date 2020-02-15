# CSS

## 목록

* [Flex](#flex)
* [z-index](#z-index)
* [VH, VW](#vh-vw)

### Flex

컨테이너와 그 안에 아이템으로 나뉘며, 컨테이너 내부의 아이템을 쉽게 배치할 수 있는 기능 제공

> `display: flex;`

#### align-items, align-content, align-self

3개가 다 비슷한 느낌이여서 헷갈렸음. 각각의 기능은

* align-items : 세로축 정렬
* align-content : `flex-wrap` 설정으로 두줄 이상인 경우의 세로축 정렬
* align-self : 각 아이템들이 `align-items` 에 의해 정렬된 개별 아이템 공간 내에서 정렬

즉, `flex-direction`, `justify-content` 설정과 함께 일반적으로 세로축 정렬에 사용하는 옵션은 `align-items` 속성이다.

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
* https://alikerock.tistory.com/270

### z-index

position 속성에 의해 아이템들끼리 겹쳐지는 경우 위에 배치할 순서를 정하는 속성
* 숫자가 클수록 상단에 오는 아이템
* 마이너스 가능

#### Sample

헤더를 고정하기 위해 `position: fixed;`를 사용했는데, 컨텐츠를 스크롤할 때 헤더를 가리는 경우가 생김.  
* z-index 속성으로 헤더를 가장 높은 값을 줘서 헤더가 가려지지 않도록 함.

```css
header {
  position: fixed;
  z-index: 999;
}
```

#### 참고

* https://homzzang.com/b/css-113

### VH VW

viewport의 높이값과 너비값의 비율 단위 (1/100)

* 높이가 1000px인 화면에서
  * 100vh == 최대값
  * 1vh == 10px
* 너비도 마찬가지
* 연산도 가능
  * `height: calc(100vh - 150px);`

#### 참고

* https://webclub.tistory.com/356
