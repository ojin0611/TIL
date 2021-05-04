# Spring Boot

Spring Boot는 Spring으로 Application 개발 시 사전에 했던 많은 작업들(library 추가, dependency 설정 등)을 줄여준다.

## 장점

- project에 따라 자주 사용되는 library들이 미리 조합돼있다. 복잡한 설정을 자동으로 처리한다.
- 내장서버를 포함하기때문에 Tomcat같은 WAS를 추가하지 않아도 개발 가능
- WAS에 배포하지않고 실행할 수 있는 JAR 파일로 Web Application을 개발할 수 있다.



## 시작 방법

1. [웹페이지에서 클릭을 통해 구성](start.spring.io)
2. STS에서 Spring Starter Project로 프로젝트 생성



## 주요 파일

## HelloSpringBootApplication.java

annotation `@SpringBootApplication` 을 사용해 main method를 실행시키는 메인 클래스다.

위 annotation을 사용하면 이 클래스가 있는 곳을 base package로 설정하여 이 패키지를 기준으로 component-scan을 실행한다. (Repository, Configuration 등..)



annotation `@EnableAspectJAutoProxy`를 main 메소드 위에 두어 aop 설정을 할 수 있다.



### application.properties

src/main/resources/

Spring의 xml에서 해주던 설정을 SpringBoot에서는 모두 application.properties에 해준다.

- ViewResolver
  - `spring.mvc.view.prefix`, `spring.mvc.view.suffix`
- Datasource
  - `spring.datasource.driver-class-name`, `url`, `username`, `password`
- MyBatis 
  - `mybatis.type-aliases-package`, `mybatis.mapper-locations`

## static

src/main/resources/

정적 리소스 directory다. (css, js, img등)



## templates

src/main/resources/

SpringBoot에서 사용 가능한 여러가지 View Template(Thymeleaf, Velocity, FreeMaker 등)의 위치다.