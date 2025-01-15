### Spring Data JPA에서 새로운 Entity인지 판단하는 방법은 무엇일까요?
```
EntityInformation의 isNew() 메서드를 통해 새로운 Entity 여부를 판단하며, 기본적으로 @Id 값이 참조 타입이면 null인지, 기본 타입이면 0인지 확인하여 persist 또는 merge를 수행한다.
ID를 직접 할당하는 경우에는 Persistable 인터페이스를 구현해 커스텀 로직으로 새로운 Entity 여부를 판단할 수 있다.
```

