### 1. root-context.xml
- 스프링의 빈으로 인식하기 위해서 root-context.xml에 @Service 어노테이션이 있는
org.zerock.service 패키지를 스캔(조사) 하도록 추가해야 함
- namespaces -> context 추가

>```xml
><context:component-scan base-package="org.zerock.service"></context:component-scan>
>```
