# passwordEncoder
해커들이 레인보우 테이블을 통해 공격을 방지하기위해 패스워드에 솔트라는 것을 적용한다.
스프링 시큐리티의 BCrypt는 내부적으로 솔트를 생성하여 패스워드와 같이 해싱하여 위 공격으로 부터 방지해준다.
---
# Spring Retry 정리

Spring Retry는 특정 예외가 발생했을 때 자동으로 재시도하는 기능을 제공한다.  
주요 애너테이션은 `@Retryable`, `@Recover`, `@EnableRetry`가 있다.

## 1. @Retryable
특정 예외가 발생하면 지정된 횟수만큼 재시도하는 애너테이션이다.

**📌 주요 속성**
- `value` : 재시도할 예외 클래스 (기본값: `Exception.class`)
- `maxAttempts` : 최대 재시도 횟수 (기본값: 3)
- `backoff` : 재시도 간격 (`@Backoff` 설정 가능)

**📌 사용 예시**
```java
@Retryable(value = RuntimeException.class, maxAttempts = 3, backoff = @Backoff(delay = 1000))
public String retryMethod() {
    // 실패 시 재시도
}
```
## 2. @Recover
특정 예외가 발생하면 복구하는 로직을 실행한다.
```java
@Recover
public String recoverMethod(RuntimeException e) {
    return "재시도 실패 후 실행되는 로직";
}
```

## 3. @EnableRetry
Spring Boot에서 @Retryable을 사용하려면 @EnableRetry를 활성화해야 한다.
이 애너테이션을 @Configuration 클래스에 선언하면 애플리케이션 전반에서 Retry 기능을 사용할 수 있다.

@Retryable이 동작하지 않는 문제가 발생해 확인해보니, @EnableRetry가 선언된 위치가 @Bean으로 등록된 클래스가 아니어서 정상적으로 동작하지 않았다.

```java
@Configuration
@EnableRetry
public class RetryConfig {
}
```
