
# ERROR
>#### 1. @Log4j
>> - @Log4j 에러는 `<scope>runtime</scope>`을 주석처리 하거나 provided로 변경
>> maven에서는 의존성 범위(scope)로 compile, provided, runtime, test, system을 제공하고 있습니다.
>> - `compile`: <scope> .. </scope>을 지정하지 않는 경우, 기본값으로 설정됩니다. 컴파일 의존성은 프로젝트의 컴파일, 테스트, 실행에 라이브러리가 필요할 때 사용합니다.
>> - `provided`: 일반적으로 JDK 또는 컨테이너가 해당 라이브러리를 제공할 때 설정합니다. 즉, 웹 애플리케이션의 경우 JSP와 Servlet API 등은 provided 의존성으로 설정합니다.
>> - `runtime`: 컴파일 시에는 사용되지 않으나, 실행과 테스트 시에는 필요할 때 설정합니다. 대표적인 예가 JDBC 드라이버입니다.
>> - `test`: 애플리케이션의 실행에는 사용하지 않으나, 테스트 컴파일 및 실행 시에 필요할 때 설정합니다. 대표적인 예로는, easymock, junit 등이 있습니다.
>> - `system`: provided 의존성과 비슷하지만, 사용자가 jar 파일의 위치를 지정한다는 점이 다릅니다. system 의존성을 사용하려면, <systemPath> .. </systemPath> 엘리먼트를 이용하여 jar 파일의 위치를 지정해야 합니다. 그러나 사용자마다 개발 환경이 다를 수 있으므로 프로퍼티를 이용하여 jar 파일의 위치를 지정하는 것이 좋습니다. 예를 들면, ${user.home}/test-1.0.jar 또는  ${basedir}/test-${os.name}-1.0.jar 등으로 설정할 수 있겠죠.

---
