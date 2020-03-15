# Spring boot

## 목록

* [Cache](#Cache)

### Cache

스프링 부트에서는 다양한 캐시 저장소를 쉽게 사용할 수 있도록 추상화 되어있다. `@Cacheable`, `@CacheEvict` 과 같이 쉽게 데이터를 캐싱하고 지울 수 있다.

#### 사용

* 의존성 추가

```groovy
implementation 'org.springframework.boot:spring-boot-starter-cache'
```

* 사용

```java
@EnableCaching
@SpringBootApplication
public class AppApplication {
	public static void main(String[] args) {
		SpringApplication.run(AppApplication.class, args);
	}
}
```

#### 주의사항

같은 클래스 내에 있는 메서드를 캐싱하는 경우 self-invocation 으로 인하여 캐싱이 되지 않는다.(자기 자신의 프록시 객체를 만들어서 프록시 객체의 메서드를 호출하기 때문에 캐시 인터셉터가 캐시 결과값을 가져와서 저장하지 못한다.) 즉, 참고 : https://stackoverflow.com/questions/16899604/spring-cache-cacheable-not-working-while-calling-from-another-method-of-the-s

나의 경우는 컨트롤러에서 서비스를 호출하고, 컨트롤러에서 호출한 서비스의 A메서드에서 같은 서비스 내에 B메서드를 호출한다. 이때, B메서드에 `@Cacheable`로 캐싱을 하려고 하는 경우 캐싱이 되지 않았다. 그래서 B메서드를 별도의 빈으로 분리한 뒤 기존 서비스에서 분리한 빈을 주입받아서 B메서드를 호출하도록 변경했다.

* 수정 전

```java
@Service
public class SampleService() {
  
  public String A(Sample vo, boolean isCache) {
    return B(vo, isCache);
  }
  
  @Cacheable(key = "#vo.id", value = "fooCache", condition = "#isCache")
  public String B(Sample vo, boolean isCache) {
    return getData();
  }
}
```

* 수정 후

```java
@Service
@RequiredArgsConstructor
public class SampleService() {
  private final ExampleUtil exampleUtil;

  public String A(Sample vo, boolean isCache) {
    return exampleUtil.B(vo, isCache);
  }
}

@Component
public class ExampleUtil() {
  @Cacheable(key = "#vo.id", value = "fooCache", condition = "#isCache")
  public String B(Sample vo, boolean isCache) {
    return getData();
  }
}
```

#### Caffeine cache

caffeine 캐시는 redis와는 다르게 별도로 저장소가 있지 않고 jvm 메모리 상에 캐싱 데이터를 저장한다. 그래서 별도로 환경을 구성할 필요 없이 자바, 스프링 환경에서 쉽게 사용할 수 있는 인메모리 캐시이다. 공식문서 : https://github.com/ben-manes/caffeine

* 의존성 추가 (버전은 https://mvnrepository.com/ 에서 참고)

```groovy
compile group: 'com.github.ben-manes.caffeine', name: 'caffeine', version: '2.8.1'
```

* 설정
```yml
spring:
  cache:
    cache-names: fooCache, barCache # 사용할 캐시 name들
    caffeine:
      spec: maximumSize=500, expireAfterAccess=180s
```
