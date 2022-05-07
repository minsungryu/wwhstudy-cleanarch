# 03 코드 구성하기

새 프로젝트에서 가장 먼저 제대로 만들려고 하는 것은 패키지 구조다.

## 계층으로 구성하기

```
buckpal
|-- domain
|  |-- Account
|  |-- Activity
|  |-- AccountRepository
|  |-- AccountService
|-- persistence
|  |-- AccountRepositoryImpl
|-- web
   |-- AccountController
```

문제점1. 애플리케이션의 기능 조각이나 특성을 구분 짓는 패키지 경계가 없다. 추가적인 구조가 없다면, 서로 연관되지 않은 기능들끼리 예상하지 못한 부수효과를 일으킬 수 있다.

문제점2. 애플리케이션이 어떤 유스케이스를 제공하는지 파악할 수 없다. 특정 기능을 찾기 위해서 어떤 서비스가 이를 구현했는지 추측해야 하고, 해당 서비스 내의 메서드도 찾아야 한다.

아키텍처를 파악할 수 없고 인커밍 포트와 아웃고잉 포트가 코드 속에 숨겨져 있다.

## 기능으로 구성하기

```
buckpal
|-- account
   |-- Account
   |-- AccountController
   |-- AccountRepository
   |-- AccountRepositoryImpl
   |-- SendMoneyService
```

애플리케이션의 기능을 코드를 통해 볼 수 있게 만드는 것을 ‘소리치는 아키텍처’라고 한다.

그러나 기능에 의한 패키징은 가시성을 떨어뜨린다.

## 아키텍처적으로 표현력 있는 패키지 구조

```
buckpal
|-- account
   |-- adapter
   |  |-- in
   |  |  |-- web
   |  |     |-- AccountController
   |  |-- out
   |  |  |-- persistence
   |  |     |-- AccountPersistenceAdapter
   |  |     |-- SpringDataAccountRepository
   |-- domain
   |  |-- Account
   |  |-- Activity
   |-- application
      |-- SendMoneyService
      |-- port
         |-- in
         |  |-- SendMoneyUseCase
         |-- out
            |-- LoadAccountPort
            |-- UpdateAccountStatePort
```

소프트웨어 개발 프로젝트에서 아키텍처는 코드에 직접적으로 매핑될 수 없는 추상적 개념이다.

‘아키텍처-코드 갭’ 혹은 ‘모델-코드 갭’이라는 용어는 이 사실을 보여준다.

패키지가 많다는 것은 public에 대한 허용이 늘어난다고 볼 수 있지만 어댑터 패키지는 포트 인터페이스를 통해서만 접근할 수 있다.(package-private)

어댑터에서 접근해야하는 포트는 public이어야 한다.

도메인 클래스도 서비스 및 어댑터에서 접근 가능하도록 public이어야 한다.

서비스는 인커밍 포트 인터페이스 뒤에 숨겨질 수 있기 때문에 public일 필요가 없다.

## 의존성 주입의 역할

웹 어댑터와 같이 인커밍 어댑터의 경우 제어 흐름의 방향이 어댑터와 도메인 코드 간의 의존성 방향과 같다.

따라서 애플리케이션이 인커밍 어댑터에 의존성을 갖지 않는다.

그러나 영속성 어댑터와 같은 아웃고잉 어댑터에 대해서는 제어 흐름의 반대 방향으로 의존성을 돌리기 위해 의존성 역전 원칙을 이용해야 한다.

그러나 포트를 구현한 실제 객체를 누가 애플리케이션 계층에 제공해야 할까? 이를 위해 모든 계층에 의존성을 가진 중립적인 컴포넌트를 도입할 수 있다. (스프링과 같은 프레임워크)

---

질문1. 그림을 보면 인터페이스와 구현체(포트와 어댑터)에 대한 설명은 잘 되어있다. 그러나 이들을 매개하는 객체는 어디에 속해야 하는가?

질문2. 멀티 모듈로 구성할 경우 고려할 점은 무엇인가?