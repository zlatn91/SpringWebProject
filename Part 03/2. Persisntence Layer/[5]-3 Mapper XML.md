# 1. Mapper XML
```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0/EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="org.zerock.mapper.BoardMapper">
  </mapper>
```
- `src/main/java`에 생성했던 `BoardMapper.java` 인터페이스의 패키지와 같은 `Dept`로 `src/main/resources` 에 생성해준다
    - `org/zerock/mapper/BoardMapper.xml`
      이때의 주의 사항은 `폴더를 한번에 생성하지말고 하나씩 생성`해야 한다는 점
    - XML 파일의 폴더 구조나 이름은 무방하지만 패키지, 클래스의 이름과 동일하게 해주면 나중에 혼란스러운 상황을 피할 수 있음
- `<mapper>` 의 `namespace` 속성값을 `Mapper 인터페이스와 동일한 이름`을 주는것에 주의
-  웹 프로젝트 구조에서 마지막 영역이 Persistence tier 이지만 실제로 구현을 가장 먼저 할 수 있는 영역도 영속 영역.
- 기본적으로 CRUD 작업을 하기 때문에 테이블과 VO(DTO) 등 약간의 준비만으로도 비지니스 로직과 무관하게 CRUD 작업을 작성할 수 있다
- MyBatis는 내부적으로 JDBC의 Prepared Statement를 활용하고 필요한 파라미터를 처리하는 `'?'에 대한 치환은 #{속성}`을 이용
---
### 2. Select
```xml
    <select id="getList" resultType="org.zerock.domain.BoardVO">
      <![CDATA[
      select * from tbl_board where bno > 0
      ]]>
    </select>
```
- select 태그안의 `id 속성은 메서드의 이름과 동일`하게 작성 !!
- `resultType` 속성의 값은 select 쿼리의 `결과를 특정 클래스의 객체`로 만들기 위해 설정
- `<![CDATA[]]>` 태그는 XML에서 `부등호(<, >, ...)` 사용을 위함
---
### 3. insert
  - 예제에선 pk칼럼을 bno로 값을 `시퀀스`를 이용했기 때문에 이처럼 자동으로 PK값이 정해지는 경우는 2가지 방식으로 처리
  - **insert만 처리되고 생성된 PK 값을 알 필요가 없는 경우**
> - ##### 1. insert  
>   - 단순히 시퀀스의 다음 값을 구해서 insert 할 때 사용
>   - insert 문은 몇 건의 데이터가 변경되었는지만을 알려주기 때문에 추가된 데이터의 PK값은 알 수 없지만
> 한번의 SQL 처리만으로 작업이 완료되는 장점이 있다
>```xml
>  <insert id="insert">
>    insert into tbl_board (bno, title, content, writer)
>    values (seq_board.nextval, #{title}, #{content}, #{writer})
>  </insert>
>```
>
  - **insert문이 실행되고 생성된 PK 값을 알아야 하는 경우**\
> - ##### 2. insertSelectKey
>   - @SelectKey를 이용하는 방식은 SQL을 한번더 실행하는 부담이 있기는 하지만 자동으로 추가되는 PK 값을 확인해야 하는 상황에서는 유용하게 쓰일 수 있다.
>   - 시퀀스 값은 현재 테스트하는 환경에 다라 다른 값이 나옴 중복 없는 값을 위한 것일 뿐 다른 의미가 있지 않다
>   -	@SelectKey라는 MyBatis의 어노테이션을 이용
>   -  주로 PK 값을 미리(before) SQL을 통해서 처리해 두고 특정한 이름으로 결과를 보관하는 방식
>   - @insert 할때 SQL문을 보면 #{bno}와 같이 미리 처리된 결과를 이용하는 것을 볼 수 있음
>```xml
>    <insert id="insertSelectKey">
>    	<selectKey keyProperty="bno" order="BEFORE" resultType="long">
>    		select seq_board.nextval from dual
>    	</selectKey>
>    	insert into tbl_board (bno, title, content, writer)
>    	values (#{bno}, #{title}, #{content}, #{writer})
>    </insert>
>```

---
## 4. read(Select)
  insert 된 데이터를 조회하는 작업은 PK를 이용함 (bno)
  BoardMapper 인터페이스에 파라미터 역시 BoardVO의 bno 타입 정보를 이용해서 처리
```xml
    <select id="read" resultType="org.zerock.domain.BoardVO">
    	select * from tbl_board where bno = #{bno}
    </select>
```
---
## 5. delete
 - insert 된 데이터를 삭제하는 경우 역시 PK값을 이용해서 처리
 - 조회(read) 작업과 유사
 - 등록, 삭제, 수정과 같은 DML 작업은 '몇 건의 데이터가 삭제 혹은 수정 되었는지를 반환 할 수 있다
```xml
  <delete id="delete">
	   delete tbl_board where bno = #{bno}
  </delete>
```
---
## 6. update
  - SQL 에서 주의 깊게 봐야할 점은 updateDate 칼럼이 최종 수정시간을 의미하는
  칼럼이기 때문에 현재 시간으로 변경해 주고 있다는 점, regdate 컬럼은 최초 생성 시간이므로 건들이지 않았다는 점
  - #{title}과 같은 부분은 파라미터로 전달된 BoardVO 객체의 getTilte()과 같은 메서드들을 호출해서 파라미터들이 처리됨
```xml
  <update id="update">
  	update tbl_board
  	set title = #{title},
  	content = #{content},
  	writer = #{writer},
  	updateDate = sysdate
  	where bno = #{bno}
  </update>
```
---
