# Spring 강좌 7
**강좌** : [7. 빈](https://youtu.be/qaIQfl0ob84)

### Bean 등록
빈의 등록은 `ioc container` 에 등록하는 것을 의미한다. 컨테이너에 등록하면 인스턴스를 컨테이너에서 생성해 준다. 주입은 빈끼리만 가능하다.
##### 생성방법
*  `ComponentScan` : `Controller`, `Service`, `Repository`, `Component`, `Configuration` 등 의 어노테이션이 추가된 클래스를 등록
	* 컴포넌트 스캔 어노테이션이 붙은 패키지 하위의 모든 클래스의 위 어노테이션을 찾아서 등록
	* 또는 인터페이스를 상속받는 것만으로도 빈이 등록됨 (`JPA`의 `Repository`)
*  `XML`  에 등록
* 자바 `class`에 등록 >> `@Configuration` 클래스에 등록한 `@Bean` 들
	* 빈에서 리턴하는 객체 자체가 빈으로 등록된다.
	* 따라서 빈으로 등록해놓은 클래스는 어노테이션이 없어도 빈으로 등록이 된다.
	* `Configuration` 도 컴포넌트스캔이 된다.
##### 사용방법
빈을 가져오는 방법
* `ApplicationContext`  의 `getBean`
* `@Autowired` >> setter injection
* 생성자 주입  `private final`
	* spring 은 생성자 주입을 권장한다. >> 생성자로 주입받아야 의존성관리가 명확해진다. 이유는 해당 빈이 없으면 생성 자체가 안되기 때문이다.

#study/spring/백기선