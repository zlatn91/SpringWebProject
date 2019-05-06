###1. 방법

- ##### MyBatis 는 `SQL` 처리를 `XML` 이나 `어노테이션` 을 이용할 수 있다
  - **SQL**
    - `간단한 SQL` 이라면 `어노테이션`으로 처리
    - `복잡` 해지고 `검색` 과 같이 `상황에 따라 다른` 경우 `어노테이션은 유용하지 못하다는 단점` 존재  
    - `어노테이션`의 경우 코드를 수정하고 다시 빌드하는 등의 `유지보수성이 떨어지는 이유`로 `기피`하는 경우도 종종 있음  
  - **XML**
    - 단순 텍스트를 수정하는 과정만으로 처리가 끝남
---
  - ##### 1. @Select, @Insert .. 어노테이션 이용
    ```java
      @Select("select * from tbl_board where bno > 0")
    ```
  - ##### 2. BoardMapper.xml 작성
    ```xml
      <select id="getList" resultType="org.zerock.domain.BoardVO">
    		<![CDATA[
    		select * from tbl_board where bno > 0
    		]]>
      </select>
    ```
---
