### RedisTamplate
`org.springframework.data.redis.core`
RedisConnection 은 row level 의 메서드를 제공하는 반면 레디스 템플릿은 커넥션 위에서 값을 조작하는 메서드 제공

### StringRedisTemplate
대부분 레디스 key-value 는 문자열 위주이기 때문에 문자열에 특화된 템플릿을 제공
RedisTemplate<String, String> 을 상속받은 클래스임.
`StringRedisSerializer`로 직렬화함.
```java
public class StringRedisTemplate extends RedisTemplate<String, String> {
//생략
}
```

*관련이슈*
`RedisTamplate` 과 `HashOperations<Object, Object, Object> hashOperations;`으로 hash를 사용하니깐 key값이나 전달한 object들의 값이 깨져서 나옴.

*해결*
`StringRedisTemplate`과 `private HashOperations<String, String, String> hashOperations;` 사용하니 직렬화가 잘된다.

*참고코드*
```java
@Bean
public RedisTemplate<Object, Object> redisTemplate(JedisConnectionFactory connectionFactory) {
	ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL, JsonTypeInfo.As.PROPERTY);
	objectMapper.enable(DeserializationFeature.ACCEPT_EMPTY_ARRAY_AS_NULL_OBJECT);
        objectMapper.registerModule(new JavaTimeModule());

        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer(objectMapper));
        redisTemplate.setConnectionFactory(connectionFactory);
        return redisTemplate;
    }

@Bean
public StringRedisTemplate stringRedisTemplate(JedisConnectionFactory connectionFactory) {
        StringRedisTemplate stringRedisTemplate = new StringRedisTemplate();
        stringRedisTemplate.setConnectionFactory(connectionFactory);

        return stringRedisTemplate;
    }
```

```java
private HashOperations<String, String, String> hashOperations;

@PostConstruct
public void init() {
	hashOperations = stringRedisTemplate.opsForHash();
}
```

### 참고
[레디스](http://arahansa.github.io/docs_spring/redis.html)