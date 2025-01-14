# RabbitMQ 설정 방법 및 사용 방법

이 문서는 RabbitMQ 설정 코드의 기능과 사용 방법에 대해 작성된 내용입니다. 각 코드 블록은 실제 설정에서 어떤 역할을 하는지 설명합니다.

---

## 개요
RabbitMQ 설정은 큐, 익스체인지, 바인딩을 정의하고, 메시지를 JSON으로 변환하기 위한 메시지 컨버터를 추가하며, 커스텀 RabbitTemplate을 설정합니다.

---

## 코드 설명

### 1. 큐 설정
```java
@Bean
public Queue orderQueue() {
    return new Queue(ORDER_QUEUE, true);
}
```
- `ORDER_QUEUE` 이름의 내구성 있는 큐를 생성합니다.
- 내구성(Durable)이 true로 설정되면 RabbitMQ가 재시작되더라도 큐가 삭제되지 않습니다.

#### 사용 목적:
이 큐는 메시지가 라우팅된 후 저장되는 대상입니다. 큐에 저장된 메시지는 소비자가 처리할 때까지 유지됩니다.

---

### 2. 익스체인지 설정
```java
@Bean
public DirectExchange orderExchange() {
    return new DirectExchange(ORDER_EXCHANGE);
}
```
- `ORDER_EXCHANGE`라는 이름의 `DirectExchange`를 생성합니다.
- DirectExchange는 특정 라우팅 키를 사용하여 큐와 메시지를 연결합니다.

#### 사용 목적:
이 익스체인지는 지정된 라우팅 키를 통해 메시지를 적절한 큐로 전달합니다.

---

### 3. 바인딩 설정
```java
@Bean
public Binding orderBinding(Queue orderQueue, DirectExchange orderExchange) {
    return BindingBuilder.bind(orderQueue).to(orderExchange).with(ORDER_ROUTING_KEY);
}
```
- `orderQueue`와 `orderExchange`를 `ORDER_ROUTING_KEY`로 연결합니다.
- 메시지가 익스체인지로 들어올 때, 라우팅 키가 `ORDER_ROUTING_KEY`와 일치하면 `orderQueue`로 전달됩니다.

#### 사용 목적:
이 바인딩 설정은 특정 라우팅 키에 따라 메시지가 적절한 큐로 전달되도록 보장합니다.

---

### 4. 메시지 컨버터 설정
```java
@Bean
public MessageConverter jsonMessageConverter() {
    return new Jackson2JsonMessageConverter();
}
```
- `Jackson2JsonMessageConverter`를 메시지 컨버터로 설정합니다.
- 메시지를 JSON 형식으로 직렬화하거나 역직렬화합니다.

#### 사용 목적:
RabbitMQ로 주고받는 메시지를 JSON 형식으로 변환하여, Java 객체를 쉽게 처리할 수 있도록 합니다.

---

### 5. RabbitTemplate 설정
```java
@Bean
public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
    RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
    rabbitTemplate.setMessageConverter(jsonMessageConverter());
    return rabbitTemplate;
}
```
- Spring Boot의 기본 ConnectionFactory를 주입받아 `RabbitTemplate`을 생성합니다.
- 메시지 컨버터를 `Jackson2JsonMessageConverter`로 설정합니다.

#### 사용 목적:
이 RabbitTemplate은 JSON 메시지를 송수신하는 데 사용됩니다. 사용자 정의 로직이 필요한 경우 적합합니다.

---

## 추가 참고 사항
1. **ConnectionFactory:**
   - 별도로 정의하지 않으면 Spring Boot는 `application.yml` 또는 `application.properties` 설정에 따라 기본 `ConnectionFactory`를 생성합니다.

2. **DirectExchange:**
   - 라우팅 키가 정확히 일치하는 경우에만 메시지를 큐로 전달합니다.

3. **내구성 설정:**
   - 큐와 익스체인지가 내구성 있게 설정되면 RabbitMQ 재시작 후에도 삭제되지 않습니다.

4. **오류 처리:**
   - 라우팅되지 않은 메시지는 DLX(Dead Letter Exchange)를 설정하거나 Alternate Exchange를 사용하지 않는 한 삭제됩니다.

---

이 설정은 RabbitMQ를 사용하는 Spring Boot 애플리케이션에서 JSON 기반 메시지를 처리하기 위한 기본적인 구성입니다.

### 6. 메시지 전송 
```java
rabbitTemplate.convertAndSend(ORDER_EXCHANGE, ORDER_ROUTING_KEY, orderDto)
```
- 파라미터에 해당하는 익스체인지와 라우팅 키를 통해 메시지를 전송합니다.
