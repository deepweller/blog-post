### 시작하며
mybatis를 사용할 때 `foreach` 태그로 쉽게 다중쿼리를 작성할 수 있다. parameter로 리스트를 받아오고 해당 리스트에 있는 VO들에 대응하는 insert 또는 update, delete 등등 다중 쿼리를 생성할 수 있다. 하지만 이렇게 다중 쿼리를 생성을 하고나면 쿼리에 syntax error 가 있다는 출력을 보게된다. 이를 해결하는 jdbc 설정도 알아본다.

### foreach
```xml
<update id="updateState" parameterType="java.util.List">
    <foreach collection="list" item="vo" index="index">
        UPDATE SAPMLE_TABLE SET state=#{vo.state} where id=#{vo.id};
    </foreach>
</update>
```
다중쿼리를 생성하는 방법은 간단하다. 위 처럼 파라미터로 리스트를 받고 `<foreach>`를 이용하면 리스트에 담긴 항목을 하나씩 꺼내서 다중 쿼리를 생성한다.   
하지만 이렇게 실행을 하게되면 에러가 나온다. 콘솔에 있는 쿼리는 분명 잘 생성이 됐고, 생성된 쿼리를 사용하고 있는 데이터베이스 툴에서 돌려봐도 문제가 없다. 이때 설정해 줘야 하는 것이 있다.

### allowmultiqueries
스프링 부트에서는 yml이나 properties 파일에 jdbc 설정을 한다. 이때 jdbc의 옵션에 `allowmultiqueries`를 추가해줘야 한다. (이 속성은 mysql에만 있다고 합니다.) 위 속성을 jdbc를 정의해 놓은 곳에 추가해 주면 위에서 생긴 에러가 해결된다.
```
spring.datasource.url=jdbc:mysql://url:3306/database?characterEncoding=UTF-8&allowMultiQueries=true
```

### 마치며
간단하게 파라미터로 리스트를 받아와 `foreach` 태그로 다중쿼리를 생성하는 방법과 jdbc 옵션에 `allowmultiqueries`를 추가하는 방법을 살펴보았다.  
  
글 주제와는 별게지만 추가로 쿼리 매핑 기반인 mybatis와 객체 매핑 기반인 jpa hibernate를 모두 사용해본 입장에서는 jpa가 좀 더 oop에 근접한 데이터베이스 엑세스 방법인 것 같다.  
먼저 쿼리 기반인 mybatis는 업무 로직을 많은부분 쿼리에서 해결하게 된다. 단순한 crud를 만드는데도 반복적인 작업이 있고, 복잡한 로직을 쿼리로 해결하게 된다. 이렇게 개발을 하면서 쿼리개발자인지 자바개발자인지 해깔릴 때가 있다. 물론 jpa가 쿼리를 전혀 안만든다는 것은 아니다. 하지만 객체 매핑만으로 orm이 자동생성해주는 쿼리보다 더 성능이 좋은 쿼리가 있다고 자신하는 경우가 아니면 거의 없다.  
또 개발단계에서는 테이블의 수정이 빈번하게 일어난다. 컬럼이 추가되거나 삭제되는 경우 관련된 쿼리를 다 찾아서 수정해줘야 한다. jpa는 객체에 속성을 하나 추가하는 것으로 끝난다.  
마지막으론 멀티 데이터베이스 지원인 경우다. oracle과 mysql에 모두 적용되는 쿼리를 작성하면 될 수 있지만 그 과정이 훨씬 귀찮고, 과연 지금 데이터베이스가 바뀔까? 하는 생각으로 현재 데이터베이스에 맞는 쿼리를 짜게 된다. 하지만 그렇게 될일이 없다고 100% 장담할 수는 없다.  
진입장벽이 높다라고 하지만 끊임없이 공부를 해야하는 개발자의 인생이라면 진입장벽이라는 단어는 핑계인 것 같다. 아직 경험이 적다보니 단순히 느껴지는 것만 적어봤다. 경력이 훨씬 많으신 분들과 mybatis와 jpa를 더 많이 더 깊게 사용해본 분들에게 어떻게 생각하시는지 직접 조언을 구해보고 싶다.