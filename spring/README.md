# Spring

[다운로드](https://spring.io/projects)

Spring Framework는 자바로 Enterprise Application을 만들 때 포괄적으로 사용하는 Programming 및 Configuration Model을 제공해주는 Framework다. Application 수준의 인프라 스트럭쳐를 제공한다.



# Spring의 구조

Enterprise Application 개발 시 복잡함을 해결하는 Spring의 핵심 4가지를 아래와 같은 삼각형으로 표현할 수 있다.

![image-20210426093415762](images/image-20210426093415762.png) 

## POJO (Plain Old Java Object) 

특정 환경이나 기술에 종속되지 않은 객체지향 원리에 충실한 (EJB 이전) 자바 객체.

테스트하기 용이하며, 객체지향 설계를 자유롭게 적용할 수 있다.



## PSA (Portable Service Abstraction)

환경과 세부기술 변경과 관계없이 일관된 방식으로 기술에 접근할 수 있게 해주는 설계 원칙.



## IoC / DI (Inversion of Control, Dependency Injection)

DI : 유연하게 확장 가능한 객체를 만들어두고 객체 간의 의존관계는 외부에서 dynamic하게 설정한다.



## AOP (Aspect Oriented Programming)

관심사(관점)의 분리를 통해 소프트웨어의 모듈성 향상

공통 모듈을 여러 코드에 쉽게 적용가능.





# Spring Framework의 특징

**경량 컨테이너**

- 스프링은 자바객체를 담고있는 컨테이너다. (Spring Container) 자바 객체의 생성,소멸같은 life cycle을 관리한다.
- 언제든지 spring container로부터 필요한 객체를 가져와 사용할 수 있다.

**DI 패턴 지원**

- 설정 파일, annotation을 통해 객체 간 의존관계를 설정할 수 있다.
- 객체는 의존하고있는 객체를 직접 생성하거나 검색할 필요가 없다.

**AOP 지원**

**POJO 지원**

**IOC**

**트랜잭션 처리를 위한 일관된 방법 제공**

**영속성 관련 다양한 API 지원**

**다양한 API에 대한 연동 지원**



# IoC & Container

객체지향 언어에서 Object간의 연결관계를 런타임에 결정한다. 객체 간의 관계가 느슨하게 연결된다.

IoC의 구현방법 중 하나가 DI.

