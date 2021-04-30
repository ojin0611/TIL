# MyBatis

MyBatis는 Java Object와 SQL문 사이의 자동 Mapping기능을 지원하는 ORM Framework다.

[MyBatis Documentation (KO)](https://mybatis.org/mybatis-3/ko/index.html)

MyBatis는 익숙한 SQL을 그대로 이용하면서 JDBC 코드 작성의 불편함을 제거해주고, 도메인 객체나 VO 객체를 중심으로 개발할 수 있게 해준다.

## 특징

- 쉬운 접근성과 코드의 간결함
  - XML 형태로 서술된 JDBC 코드. 복잡한 JDBC 코드를 걷어내어 깔끔한 소스코드 유지
  - 가장 간단한 persistence framework (데이터베이스와의 연동되는 시스템을 빠르게 개발하고 안정적인 구동을 보장해주는 프레임워크)

- SQL문과 프로그래밍 코드의 분리
  - SQL 작성과 관리 또는 검토를 DBA같은 (개발자가 아닌) 다른 사람에게 맡길 수 있다.
- 다양한 프로그래밍 언어로 구현할 수 있다.



# 사용

## MyBatis

편의상 mybatis-spring 사용법을 알아보자.

우선 mybatis, mybatis-spring 의존성 주입을 해준다. (pom.xml)

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.3</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.3</version>
</dependency>

```

root-context.xml에 MyBatis를 통해 사용할 mapper 파일들을 등록한다. SqlSession sqlSession을 Autowired로 가져오면, sqlSessionFactory의 property에 있는 mapper들에 있는 값을 불러올 수 있다. 

```xml
<!-- MyBatis-Spring 설정 -->
<bean id="ds" class="org.springframework.jndi.JndiObjectFactoryBean">
    <property name="jndiName" value="java:comp/env/jdbc/ssafy"></property>
</bean>

<!-- sqlSessionFactory -->
<bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="ds"></property>
    <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    <property name="mapperLocations">
        <list>
            <value>classpath:member.xml</value>
            <value>classpath:guestbook.xml</value>
        </list>
    </property>
</bean>

<!-- sqlSession 객체 생성 -->
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg ref="sqlSessionFactoryBean"></constructor-arg>
</bean>

```



mybatis-config.xml에 mapper에서 사용할 DTO의 별칭을 등록한다.

```xml-dtd
<configuration>
	<typeAliases>
		<typeAlias type="com.ssafy.guestbook.model.GuestBookDto" alias="guestbook" />
		<typeAlias type="com.ssafy.guestbook.model.MemberDto" alias="member" />
	</typeAliases>
</configuration>
```

각각의 mapper에는 Dao Interface를 구현한 클래스의 메소드명을 불러왔을 때 실행시킬 sql문이 존재한다. 

메소드명(id)과 paremeterType과 resultType을 명시해준다. 아래의 member는 위의 mybatis-config.xml에 등록한 alias를 불러온다.

```xml-dtd
<mapper namespace="com.ssafy.guestbook.model.mapper.UserDao">
	<select id="login" parameterType="map" resultType="member">
		select username, userid, email
		from ssafy_member
		where userid = ${userid} and userpwd = #{userpwd}
	</select>
</mapper>
```



Spring의 Service에서 위의 Dao를 implements한 클래스의 코드를 사용하고싶을 땐, sqlSession을 이용한다. sqlSession에 등록된 mapper를 가져오고, mapper에 있는 메소드를 불러온다.

```java
@Service
public class UserServiceImpl implements UserService {
	
	@Autowired
	private SqlSession sqlSession;
	
	@Override
	public MemberDto login(Map<String, String> map) throws Exception {
		if(map.get("userid") == null || map.get("userpwd") == null)
			return null;
		return sqlSession.getMapper(UserDao.class).login(map);
	}
}
```



## JNDI

JNDI (Java Naming and Directory Interface)는 디렉토리 서비스에서 제공하는 데이터나 객체를 발견하고 참고하기 위한 자바 API다. DB Pool을 미리 Naming시켜놓는 방법으로, 외부에 있는 객체를 가져오기위한 기술이다.

root-context.xml 상단에 JNDI Object를 가져오는 부분이 있다. WAS 부팅 시 JNDI 객체를 등록한다.

```xml
<bean id="ds" class="org.springframework.jndi.JndiObjectFactoryBean">
    <property name="jndiName" value="java:comp/env/jdbc/ssafy"></property>
</bean>
```



META-INF의 context.xml에 JNDI로 가져오려는 Database Pool을 등록한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>
	<Resource name="jdbc/ssafy" auth="Container" type="javax.sql.DataSource" 
			maxTotal="100" maxIdle="30" maxWaitMillis="10000" 
			username="ssafy" password="ssafy" driverClassName="com.mysql.cj.jdbc.Driver" 	
			url="jdbc:mysql://localhost:3306/ssafyweb?serverTimezone=UTC&amp;useUniCode=yes&amp;characterEncoding=UTF-8"/> 
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
</Context>
```



## Transaction Manager

commit, rollback 등을 자동으로 관리해주는 기능을 dataSource에 추가한다.

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="ds" />
</bean>

<tx:annotation-driven transaction-manager="transactionManager"/>

```

