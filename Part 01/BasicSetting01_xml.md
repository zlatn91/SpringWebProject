### 1. GENERAL, HTML, CSS, JSP

	Preferences > General > Workspace > UTF-8
	Preferences > General > Web > CSS, HTML, JSP > UTF-8

### 2. pom.xml
```xml
<!--spring-framework-->
	<properties>
		<java-version>1.6</java-version>
		<org.springframework-version>5.0.7.RELEASE</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>

<!--maven-comiler-plugin-->
	<plugin>
		 <groupId>org.apache.maven.plugins</groupId>
		 <artifactId>maven-compiler-plugin</artifactId>
		 <version>3.5.1</version>
		 <configuration>
			  <source>1.8</source>
			  <target>1.8</target>
			  <compilerArgument>-Xlint:all</compilerArgument>
			  <showWarnings>true</showWarnings>
			  <showDeprecation>true</showDeprecation>
		 </configuration>
	</plugin>

<!-- https://mvnrepository.com/artifact/log4j/log4j -->
	<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
	</dependency>
<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
	<dependency>
	    <groupId>javax.servlet</groupId>
	    <artifactId>javax.servlet-api</artifactId>
	    <version>3.1.0</version>
	    <scope>provided</scope>
	</dependency>
```
- Project > Properties > Project Facets > Java 1.8 (change) and java Compiler 1.8 (check)

- Maven > Update Project

- JRE System Library [JavaSE-1.8] check!

### 3. Server
	Run As > Run on Server
	localhost:8080/xxx/
