**스프링의 버전과 Java 버전 수정**
```xml
  <properties>
    <java-version>1.8</java-version>
    <org.springframework-version>5.0.7.RELEASE</org.springframework-version>
    <org.aspectj-version>1.6.10</org.aspectj-version>
    <org.slf4j-version>1.6.6</org.slf4j-version>
  </properties>
```
---
**스프링 관련 추가 라이브러리**
  - spring-tx, spring-jdbc, spring-test (junit 하단 위치)
```xml
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${org.springframework-version}</version>
  </dependency>
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${org.springframework-version}</version>
  </dependency>
     <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>${org.springframework-version}</version>
  </dependency>
```
---
**MyBatis를 이용하기 때문에 추가해야되는 것들**
  - HikariCP, MyBatis, mybatis-spring, Log4jdbc 라이브러리 추가 (spring-tx 하단 위치)
```xml
  <dependency>
  <groupId>com.zaxxer</groupId>
  <artifactId>HikariCP</artifactId>
  <version>2.7.8</version>
  </dependency>

  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
  <dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.4.6</version>
  </dependency>

  <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
  <dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.3.2</version>
  </dependency>

  <dependency>
  <groupId>org.bgee.log4jdbc-log4j2</groupId>
  <artifactId>log4jdbc-log4j2-jdbc4</artifactId>
  <version>1.16</version>
  </dependency>
```
---
**테스트와 Lombok을 위해서**
  - jUnit 버전을 변경
  - Lombok 추가 (Junit의 경우 4.7로 설정되어 있으므로 반드시 기존 설정을 변경하도록)
```xml
  <dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.0</version>
  <scope>provided</scope>
  </dependency>

  <!-- Test -->
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
  </dependency>
```
---
**5. Servlet 3.1 (혹은 3.0)을 제대로 사용하기 위해서**
  - pom.xml에 있던 서블릿 2.5 버전이 아닌 3.0 이상으로 수정
```xml
  <!-- Servlet -->
  <dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
  </dependency>
```
---
**Servlet 3.1 버전을 제대로 활용하고, JDK8의 기능을 활용하기 위해서**
  - Maven 관련 java 버전을 1.8로 수정
```xml
  <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>2.5.1</version>
      <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <compilerArgument>-Xlint:all</compilerArgument>
          <showWarnings>true</showWarnings>
          <showDeprecation>true</showDeprecation>
      </configuration>
  </plugin>
```
---
**프로젝트 선택 후 Maven -> Update Project (option + f5)**
- 1. Server -> Modules -> Path -> '/'
- 2. Project -> Properties(command + i) -> Web Project Settings -> Context root -> '/'
- 3. 서버 실행 -> URL: localHost:8080/
---
