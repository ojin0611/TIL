[TOC]



# Spring Web MVC

Spring은 Servlet 기반의 Web 개발을 위한 MVC Framework를 제공한다.

Spring MVC는 Model2 Architecture와 Front Controller Pattern을 Framework 차원에서 제공한다.

Spring MVC Framework는 Spring을 기반으로 하고있기 때문에 Spring이 제공하는 transaction 처리, DI, AOP 등을 손쉽게 사용할 수 있다.



### Model 2 요청 흐름

![image-20210427092304802](images/image-20210427092304802.png)



## 구성 요소

**DispatcherServlet (Front Controller)**

모든 클라이언트의 요청을 전달받는다. DispatcherServlet은 Controller에게 요청을 전달하고, Controller가 리턴한 결과값을 View에게 전달해 알맞은 응답을 생성한다.

**HandlerMapping**

클라이언트의 요청 url을 어떤 controller가 처리할지 결정한다.

url과 요청 정보를 기준으로 어떤 핸들러객체를 사용할지 결정하는 객체이며, DispatcherServlet은 하나 이상의 HandlerMapping을 가질수 있다.

**Controller**

클라이언트의 요청을 처리한 뒤, Model을 호출하고 그 결과를 DispatcherServlet에 알려준다.

**ModelAndView**

Controller가 처리한 데이터(DTO) 및 화면(View 이름)에 대한 **정보를 보유한 객체**다.

**ViewResolver**

Controller가 리턴한 View 이름을 기반으로 Controller의 처리결과를 보여줄 View를 결정한다. 논리적 이름을 실제 jsp이름으로 변환해주는 역할이다.

**View**

Controller의 처리결과를 보여줄 응답화면을 생성한다.



### Spring MVC 요청 흐름

![image-20210427094557743](images/image-20210427094557743.png) 

## 구현

1. web.xml에 DispatcherServlet 등록 및 Spring 설정파일 등록
2. 설정파일에 HandlerMapping 설정
3. Controller 구현 및 Context 설정파일(servlet-context.xml)에 등록
4. Controller와 JSP 연결을 위해 ViewResolver 설정
5. JSP 코드 작성.

Controller가 많은 일을 하지 않고 Service에 처리를 위임하는 것이 좋은 디자인이다.







## View

Spring에서는 jsp 파일을 주로 WEB-INF에 두는데 (기존 backend 코딩시에는 webapp에 배치), 이는 jsp 페이지를 url로 직접 실행하는 것을 막고 Controller를 통해서만 실행하게 하기 위해서다.

WEB-INF는 HTTP 프로토콜로는 접근이 불가능하다. web.xml 등 중요한 파일들이 다 여기 존재한다.



webapp에 배치해도 에러가 생기진 않는다. url로 jsp 파일을 실행시킬 수 있다.

![image-20210427104720405](images/image-20210427104720405.png) 

