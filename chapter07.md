# 07 아키텍처 요소 테스트하기

## 테스트 피라미드

기본 전제는 만드는 비용이 적고, 유지보수하기 쉽고, 빨리 실행되고, 안정적인 작은 크기의 테스트들에 대해 높은 커버리지를 유지해야 한다는 것이다.

단위 테스트는 일반적으로 하나의 클래스를 인스턴스화하고 해당 클래스의 인터페이스를 통해 기능들을 테스트한다.

통합 테스트는 연결된 여러 유닛을 인스턴스화하고 시작점이 되는 클래스의 인터페이스로 데이터를 보낸 후 유닛들의 네트워크가 기대한대로 잘 동작하는지 검증한다.

## 단위 테스트로 도메인 엔티티 테스트하기

테스트 코드 예제 [링크](https://github.com/wikibook/clean-architecture/blob/main/src/test/java/io/reflectoring/buckpal/account/application/domain/AccountTest.java)

## 단위 테스트로 유스케이스 테스트하기

테스트 코드 예제 [링크](https://github.com/wikibook/clean-architecture/blob/main/src/test/java/io/reflectoring/buckpal/account/application/service/SendMoneyServiceTest.java)

테스트에서 어떤 상호작용을 검증하고 싶은지 신중하게 생각해야 한다.

모든 동작을 검증하는 대신 중요한 핵심만 고랄 집중해서 테스트하는 것이 좋다.

(추천 도서: [단위 테스트](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791161755748&orderClick=LAG&Kc=))

## 통합 테스트로 웹 어댑터 테스트하기

테스트 코드 예제 [링크](https://github.com/wikibook/clean-architecture/blob/main/src/test/java/io/reflectoring/buckpal/account/adapter/in/web/SendMoneyControllerTest.java)

입력을 JSON에서 `SendMoneyCommand` 객체로 매핑하는 전 과정을 다루고 있다.

왜 통합 테스트일까?

이 테스트에서 하나의 웹 컨트롤러 클래스만 테스트한 것처럼 보이지만, 사실 보이지 않는 곳에서 더 많은 일들이 벌어지고 있다.

`@WebMvcTest` 애너테이션은 스프링이 특정 요청 경로, 자바와 JSON 간의 매핑, HTTP 입력 검증 등에 필요한 전체 객체 네트워크를 인스턴스화하도록 만든다.

웹 컨트롤러가 스프링 프레임워크에 강하게 묶여 있기 때문에 격리된 상태로 테스트하기 어렵다.

그래서 프레임워크와 통합된 상태로 테스트하는 것이 합리적이다.

## 통합 테스트로 영속성 어댑터 테스트하기

테스트 코드 예제 [링크](https://github.com/wikibook/clean-architecture/blob/main/src/test/java/io/reflectoring/buckpal/account/adapter/out/persistence/AccountPersistenceAdapterTest.java)

데이터베이스를 모킹하지 않고 실제 데이터베이스에 접근한다.

testcontainers 같은 라이브러리는 필요한 데이터베이스를 도커 컨테이너에 띄울 수 있기 때문에 이런 측면에서 아주 유용하다.

## 시스템 테스트로 주요 경로 테스트하기

테스트 코드 예제 [링크](https://github.com/wikibook/clean-architecture/blob/main/src/test/java/io/reflectoring/buckpal/SendMoneySystemTest.java)

다른 시스템과 통신해야할 경우 모킹을 해야할 수 있다.

도메인 특화 언어는 어떤 테스트에서도 유용하지만 시스템 테스트에서는 더욱 의미를 가진다.

시스템 테스트로 단위 테스트와 통합 테스트가 발견하지 못하는 계층 간 매핑 버그 같은 것들을 발견할 수 있다. 

## 얼마만큼의 테스트가 충분할까?

라인 커버리지는 테스트 성공을 측정하는 데 있어서 잘못된 지표다.

심지어 100%라 하더라도 버그가 잘 잡혔는지 확신할 수 없다.

각각의 프로덕션 버그에 대해서 "테스트가 이 버그를 왜 잡지 못했을까?"를 생각하고 이에 대한 답변을 기록하고, 이 케이스를 커버할 수 있는 테스트를 추가해야 한다.

우리가 만들어야 할 테스트를 정의하는 전략으로 시작하는 것도 좋다.

다음은 육각형 아키텍처에서 사용하는 전략이다.

- 도메인 엔티티를 구현할 때는 단위 테스트로 커버하자
- 유스케이스를 구현할 때는 단위 테스트로 커버하자
- 어댑터를 구현할 때는 통합 테스트로 커버하자
- 사용자가 취할 수 있늨 중요 애플리케이션 경로는 시스템 테스트로 커버하자

리팩터링할 때마다 테스트 코드도 변경해야 한다면 테스트는 테스트로서의 가치를 잃는다.

---

질문1. 