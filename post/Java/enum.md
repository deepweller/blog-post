### 시작하며
지금 개발하고 있는 거래,결제 파트에서 주문의 상태를 Enum으로 관리하고 있다. 거래를 중개하는 결제가 있어 유독 주문의 상태가 다양하다. 각각 주문의 상태는 구매자와 판매자의 행위에 따라 변경하게 되는데 이 때 중요하게 고려해야할 부분이 생겼다. 특정 상태에서 변경할 수 있는 상태가 정해져 있다는 점이다. 예를 들면 결제완료 상태에서 변경될 수 있는 상태는 배송중, 거래취소 등의 상태가 있다. 이 외에 다른 상태로 변경을 하려고 하면 막아야 한다. (기존 운영하던 서비스에서 이 체크가 제대로 이뤄지고 있지 않아서 큰 장애가 발생했었다.)  
이를 수많은 if, else if 로 처리해야하나 했지만 예전에 봤던 좋은 Enum 활용 글을 보고 그 방법을 사용해야 겠다고 생각했다. ([참고글](http://woowabros.github.io/tools/2017/07/10/java-enum-uses.html))

### 샘플코드
실제로 사용되는 코드가 아닌 포스팅을 위해 간단하게 작성한 코드이다. 아래는 상태를 정의한 Enum이다.
```java
@AllArgsConstructor
public enum OrderStatusChange {

    PAY("결제완료"),
    SHIPPING("배송중"),
    CANCEL("취소"),
    RETURN("환불")
    ;

    @Getter
    private String description;
}
```
아래는 상태별 변경이 가능한 목록을 정의한 Enum이다.
```java
@AllArgsConstructor
public enum OrderStatusChange {

    PAY("결제완료", Arrays.asList(OrderStatus.CANCEL, OrderStatus.SHIPPING)),
    SHIPPING("배송중", Arrays.asList(OrderStatus.CANCEL, OrderStatus.RETURN))
    ;

    @Getter
    private String description;

    @Getter
    private List<String> changableList;
}
```
간단하게 설명하면 거래상태에는 결제완료, 배송중, 취소, 환불 이렇게 4가지 상태가 있다. 그리고 결제완료 상태에서 변경이 가능한 상태로는 배송중과 취소가 있고, 배송중인 상태에서는 취소와 환불로 상태변경이 가능하다. 이때, 결제완료 상태에서 환불 상태로 변경하려고 하거나, 배송중인 상태에서 결제완료 상태로 변경하는 것은 막아야 한다.  
코드만 봐도 상당히 직관적으로 표현되어 있다. 다양한 상태가 있고 그 상태에 포함된 상태가 리스트로 되어있다. 추후에 변경 가능한 상태가 추가되거나 삭제가 되더라도 리스트에 넣거나 빼기만 하면 된다.
이제 이를 체크하는 로직을 간단하게 보면 다음과 같다.
```java
public boolean checkChangable(OrderStatus changeStatus, String nowStatus) {
    boolean changeable = false;

    if (OrderStatusChange.valueOf(nowStatus).changableList().contains(changeStatus)) {
        changeable = true;
    }

    return changeable;
}
```
함수의 파라미터로 변경하려는 상태(changeStatus)와 현재상태(nowStatus)를 차례로 받는다. 그리고 현재상태에서 변경할 수 있는 상태리스트(changableList)에 변경하려고 하는 상태가 들어있는지 확인(contatins)한다.

### 마치며
Enum을 잘 활용하면 누가 봐도 이해하기 쉽고, 관리하기 쉬운 직관적인 코드를 작성할 수 있는 것 같다. 상태의 관리부터 체크하는 로직까지 간단하게 구현이 가능하다. 실제 코드도 샘플코드와 별반 차이가 없다. 하지만 이 간단한 내용으로 이번에 발생한 장애는 다시 발생하지 않을 수 있게 됐다. 또 계속해서 누가봐도 알기 쉬운 코드르 작성하기 위한 방법을 잘 찾아봐야 겠다.

### 참고
http://woowabros.github.io/tools/2017/07/10/java-enum-uses.html  