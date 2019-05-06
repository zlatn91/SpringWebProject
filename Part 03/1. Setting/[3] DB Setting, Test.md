###[1. DataSource 와 MyBatis 연결이 반드시 필요함](#1)
---
### 데이터베이스 관련 설정 및 테스트

  - **1. root-context.xml -> Namespaces -> mybatis-spring 추가**
  - **2. DataSources, MyBatis 설정 추가**
    ```xml
      <bean id="hikariConfig" class="com.zaxxer.hikari.HikariConfig">
        <property name="driverClassName"
          value="net.sf.log4jdbc.sql.jdbcapi.DriverSpy"></property>
        <property name="jdbcUrl"
          value="jdbc:log4jdbc:oracle:thin:@localhost:1521:XE"></property>
        <property name="username" value="book_ex"></property>
        <property name="password" value="001219"></property>
      </bean>

      <!-- HikariCP configuration -->
      <bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource"
      destroy-method="close">
        <constructor-arg ref="hikariConfig" />
      </bean>

      <bean id="sqlSessionFactory"
      class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
      </bean>
    ```
  - **3. 내부적으로 Log4jdbc를 이용하는 방식으로 구성되어 있으므로 log4jdbc.log4j2.properties 파일 추가**
    - log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
---
### 1
**DataSourceTests.java**
  ```java
  package org.zerock.persistence;

  import static org.junit.Assert.fail;

  import java.sql.Connection;

  import javax.sql.DataSource;

  import org.apache.ibatis.session.SqlSession;
  import org.apache.ibatis.session.SqlSessionFactory;
  import org.junit.Test;
  import org.junit.runner.RunWith;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.test.context.ContextConfiguration;
  import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

  import lombok.Setter;
  import lombok.extern.log4j.Log4j;

  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
  //Java Config
  //@ContextConfiguration(classes= {RootConfig.class})
  @Log4j
  public class DataSourceTests {

    @Setter(onMethod_ = { @Autowired })
    private DataSource dataSource;

    @Setter(onMethod_ = { @Autowired })
    private SqlSessionFactory sqlSessionFactory;

    @Test
    public void testMyBatis() {
      try (SqlSession session = sqlSessionFactory.openSession();
         Connection con = session.getConnection();
        ) {
        log.info(session);
        log.info(con);
      } catch (Exception e) {
        fail(e.getMessage());
      }
    }

    @Test
    public void testConnection() {
      try (Connection con = dataSource.getConnection()){
        log.info(con);      
      }catch(Exception e) {
        fail(e.getMessage());
      }
    }
  }  
  ```

**JDBCTest**
  ```java
  package org.zerock.persistence;

  import static org.junit.Assert.fail;

  import java.sql.Connection;
  import java.sql.DriverManager;

  import org.junit.Test;

  import lombok.extern.log4j.Log4j;

  @Log4j
  public class JDBCTests {
  	static {
  		try {
  			Class.forName("oracle.jdbc.driver.OracleDriver");
  		} catch (Exception e) {
  			e.printStackTrace();
  		}
  	}
  	@Test
  	public void testConnection() {
  		try (Connection con = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:XE", "book_ex",
  				"001219")) {
  			log.info(con);
  		} catch (Exception e) {
  			fail(e.getMessage());
  		}
  	}
  }
  ```
---
### 2. Mapper 테스트
- 결과가 SQL Developer의 결과와 같아야 정상적으로 동작한 것
---
