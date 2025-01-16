### Spring Data JPA에서 새로운 Entity인지 판단하는 방법은 무엇일까요?
```
EntityInformation의 isNew() 메서드를 통해 새로운 Entity 여부를 판단하며,
기본적으로 @Id 값이 참조 타입이면 null인지, 기본 타입이면 0인지 확인하여 persist 또는 merge를 수행한다.
ID를 직접 할당하는 경우에는 Persistable 인터페이스를 구현해 커스텀 로직으로 새로운 Entity 여부를 판단할 수 있다.
```
### JPA의 ddl-auto 옵션은 각각 어떤 동작을 하고 어떤 상황에서 사용해야 할까요?
```
ddl-auto 옵션은 jpa에서 스키마 관리를 제어하는 옵션입니다.
none : 스키마와 관련된 어떠한 작업도 수행하지 않습니다.
create : application이 시작될 때 기존 Entity매핑과 일치하는 스키마를 삭제하고 새롭게 생성합니다.
update : application이 시작될 때 Entity에 변경된 부분들을 스키마에 적용합니다.
validate : Entity 매핑과 데이터베이스 스키마와 일치하는지 검증합니다.
create-drop : application이 시작될 때 스키마를 생성하고 종료될 때 스키마를 삭제합니다.
none과 validate는 주로 프로덕션 환경에서 사용되며,
나머지는 개발 초기 단계에서 주로 사용됩니다.
```
