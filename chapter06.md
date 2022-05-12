# 06 영속성 어댑터 구현하기

## 의존성 역전

영속성 어댑터는 '주도되는' 혹은 '아웃고잉' 어댑터다.

애플리케이션에 의해 호출될 뿐, 애플리케이션을 호출하지 않기 때문이다.

## 영속성 어댑터의 책임

1. 입력을 받는다.
2. 입력을 데이터베이스 포맷으로 매핑한다.
3. 입력을 데이터베이스로 보낸다.
4. 데이터베이스 출력을 애플리케이션 포맷으로 매핑한다.
5. 출력을 반환한다.

핵심은 영속성 어댑터의 입출력 모델이 영속성 어댑터 내부에 있는 것이 아니라 애플리케이션 코어에 있기 때문에
영속성 어댑터 내부를 변경하는 것이 코어에 영향을 미치지 않는다는 것이다. 

## 포트 인터페이스 나누기

특정 엔티티가 필요로 하는 모든 데이터베이스 연산을 하나의 리포지터리 인터페이스에 넣어둔다면 '넓은' 포트 인터페이스 의존성을 갖게 된다.

이는 코드를 이해하고 테스트하기 어렵게 만든다.

인터페이스 분리 원칙(Interface Segregation Principle, ISP)은 이 문제의 답을 제시한다.
(그림 6.3)

매우 좁은 포트를 만드는 것을 코딩을 플러그 앤 플레이 경험으로 만든다.

## 영속성 어댑터 나누기

모든 영속성 포트를 구현하는 한, 하나 이상의 클래스 생성을 금지하는 규칙은 없다.

도메인 코드는 영속성 포트에 의해 정의된 명세를 어떤 클래스가 충족시키는지에 관심이 없다.

모든 포트가 구현돼 있기만 한다면 영속성 계층에서 하고 싶은 어떤 작업이든 해도 된다.

'애그리거트당 하나의 영속성 어댑터' 접근 방식은 여러 바운디드 컨텍스트의 영속성 요구사항을 분리하기 위한 좋은 토대가 된다.

## 스프링 데이터 JPA 예제

> 코드는 다음 링크들을 참고한다. 
> [Entity](https://github.com/wikibook/clean-architecture/blob/main/src/main/java/io/reflectoring/buckpal/account/adapter/in/web/SendMoneyController.java)
> [Repository](https://github.com/wikibook/clean-architecture/blob/main/src/main/java/io/reflectoring/buckpal/account/adapter/out/persistence/ActivityRepository.java)
> [Adapter](https://github.com/wikibook/clean-architecture/blob/main/src/main/java/io/reflectoring/buckpal/account/adapter/out/persistence/AccountPersistenceAdapter.java)

## 데이터베이스 트랜잭션은 어떻게 해야 할까?

트랜잭션은 하나의 특정한 유스케이스에 대해서 일어나는 모든 쓰기 작업에 걸쳐있어야 한다.
그래야 그중 하나라도 실패할 경우 다 같이 롤백될 수 있기 때문이다.

영속성 어댑터는 어떤 데이터베이스 연산이 같은 유스케이스에 포함되는지 알지 못하기 때문에 언제 트랜잭션을 열고 닫을지 결정할 수 없다.

* 읽어볼만한 글: [Transactionless](https://martinfowler.com/bliki/Transactionless.html?fbclid=IwAR2InuG501G26XiicMEbT0fzBef60H-WwppwwviqPgn--lU6oynt8eFIkaw)
