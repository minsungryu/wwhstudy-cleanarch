# 01 계층형 아키텍처의 문제는 무엇일까?

## 계층형 아키텍처는 데이터베이스 주도 설계를 유도한다

우리의 애플리케이션은 상태가 아니라 행동을 중심으로 모델링한다.

상태는 중요한 요소이긴 하지만 행동이 상태를 바꾸는 주체이기 때문이다.

우리는 도메인 로직을 먼저 만들고 이를 기반으로 영속성 계층과 웹 계층을 만들어야 한다.

ORM 프레임워크를 사용하면 엔티티를 영속성 계층에 두기 때문에 도메인 계층과 영속성 계층 사이에 강한 결합이 생긴다.

## 지름길을 택하기 쉬워진다

계층형 아키텍처에서는 같은 계층에 있거나 아래 계층에만 접근이 가능하다.

만약 상위 계층에 위치한 컴포넌트에 접근해야 한다면 계층 아래로 내려버리면 해결된다.

처음이 힘들지 그 다음부터는 죄책감이 훨씬 덜하다.

이로인해 영속성 계층이 비대해진다.

## 테스트하기 어려워진다

웹 계층에서 바로 영속성 계층에 접근하면 두 가지 문제가 생긴다.

첫 번째, 도메인 로직이 웹 계층에 구현됨으로인해 책임이 섞이고 핵심 도메인 로직이 퍼져나갈 확률이 높다.

두 번째, 웹 계층을 테스트할 때 도메인 계층뿐만 아니라 영속성 계층도 모킹해야 한다. 이로인해 테스트 복잡도가 올라가고 테스트를 작성하지 않게 된다.

## 유스케이스를 숨긴다

계층형 아키텍처는 도메인 서비스의 ‘너비’에 관한 규칙을 강제하지 않는다. 그렇기 때문에 시간이 지나면 여러 개의 유스케이스를 담당하는 아주 넓은 서비스가 만들어지기도 한다.

넓은 서비스는 영속성 계층에 많은 의존성을 갖게 되고, 웹 계층에서도 많은 컴포넌트가 이 서비스에 의존하게 된다. 이는 서비스를 테스트하기 어렵게 만들고 작업 대상을 찾기도 어려워진다.

## 동시 작업이 어려워진다

새로운 유스케이스를 추가한다고 했을 때 계층형 아키텍처에서 웹 계층, 도메인 계층, 영속성 계층을 서로 다른 개발자가 개발할 수 없다.

---
