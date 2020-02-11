# Spring Cloud

## Feign / Hystrix

### 개요

* Feign : http(rest) 기반 서비스 호출을 추상화한 spirng cloud 라이브러리
* Hystrix : 방화벽역할

### 의존성추가

- spring boot 2.*

```groovy
dependencyManagement {
  imports {
    mavenBom 'org.springframework.cloud:spring-cloud-dependencies:Greenwich.RELEASE'
  }
}

dependencies {
    compile 'spring.framework.cloud:spring-cloud-starter-openfeign'
}
```

- spring boot 1.*

```groovy
dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:Dalston.SR1"
    }
}

dependencies {
  compile 'org.springframework.cloud:spring-cloud-starter-feign'
  compile 'org.springframework.cloud:spring-cloud-starter-hystrix'
}
```

### 샘플

```java
@EnableFeignClients
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

```java
@FeignClient(name = "service", url = "http://localhost:8080", fallbackFactory = FeignServiceImpl.class)
public interface FeignService {
    @GetMapping(value = "/ex/v1/item/{code}")
    List<Item> getList(@PathVariable(value = "code") String code);
}
```

```java
@Component
public class FeignServiceImpl implements FallbackFactory<FeignService> {
    @Override
    public FeignService create(Throwable cause) {
        return new FeignService() {
            @Override
            public List<Item> getList(String code) {
                log.error("FeignService.getList : code={}, cause.getMessage={}", code, cause.getMessage());
                return null;
            }
        };
    }
}
```

### 참고

* https://spring.io/projects/spring-cloud-openfeign
* https://supawer0728.github.io/2018/03/11/Spring-Cloud-Feign/
* https://velog.io/@skyepodium/2019-10-06-1410-%EC%9E%91%EC%84%B1%EB%90%A8
