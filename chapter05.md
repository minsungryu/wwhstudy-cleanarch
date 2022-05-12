# 05 웹 어댑터 구현하기

## 의존성 역전

(그림 5.1, 그림 5.2)

웹 어댑터는 '주도하는' 혹은 '인커밍' 어댑터다.

외부로부터 요청을 받아 애플리케이션 코어를 호출하고 무슨 일을 해야하는지 알려준다.

애플리케이션은 웹 어댑터가 통신할 수 있는 특정 포트를 제공한다.

서비스는 이 포트를 구현하고, 웹 어댑터는 이 포트를 호출할 수 있다.

자세히 보면 이곳에 **의존성 역전 원칙**이 적용된 것을 발견할 수 있다.

포트는 외부 세계와 통신하는 명세이기 때문에 유지보수할 때 도움이 된다.

## 웹 어댑터의 책임

1. HTTP 요청을 자바 객체로 매핑
2. 권한 검사
3. 입력 유효성 검증
4. 입력을 유스케이스의 입력 모델로 매핑
5. 유스케이스 호출
6. 유스케이스의 출력을 HTTP로 매핑
7. HTTP 응답을 반환

유스케이스의 입력 모델 유효성 검증과 달리 웹 어댑터의 입력 모델을 유스케이스의 입력 모델로 변환할 수 있다는 것을 검증해야 한다.

웹 어댑터에 책임이 많지만 애플리케이션 계층이 신경쓰면 안 되는 것들이기도 하다.

## 컨트롤러 나누기

웹 어댑터는 한 개 이상의 클래스로 구성해도 된다.

하지만 클래스들이 같은 소속이라는 것을 표현하기 위해 같은 패키지 수준에 놓아야 한다.

컨트롤러는 적은 것보다 많은 게 낫고 가능한 좁아야 한다.

> 코드는 다음 [링크](https://github.com/wikibook/clean-architecture/blob/main/src/main/java/io/reflectoring/buckpal/account/adapter/in/web/SendMoneyController.java)를 참고한다.

---

질문1. 