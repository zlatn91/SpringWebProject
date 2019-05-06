## 1. Mapper 인터페이스

- org.zerock.mapper.BoardMapper
- root-context.xml 추가
```xml
  <mybatis-spring:scan base-package="org.zerock.mapper"/>
```
- 인터페이스 작성시 select(리스트), insert(등록) 작업을 우선해서 생성
- 이미 작성된 BoardVO를 적극적으로 활용해서 필요한 SQL을 어노테이션의 속성값으로 처리할 수 있다
```java
    @Select("select * from tbl_board where bno > 0")
```
- SQL 작성시 반드시 ';'이 없도록
---

### 2. 메서드
- `public List<BoardVO> getList();`
- `public void insert(BoardVO board);`
- `public void insertSelectKey(BoardVO board);`
- `public BoardVO read(Long bno);`
- `public int delete(Long bno);`
    - 메서드의 리턴타입은 int로 지정해 정상적으로 데이터가 삭제 되었다면 1 이상의 값을 가지도록 작성함
    - 만일 해당번호의 게시물이 없다면 0 으로 표시
    ```
      Result
      INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
      INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
      INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
      INFO : jdbc.audit - 1. PreparedStatement.close() returned
      INFO : jdbc.audit - 1. Connection.clearWarnings() returned
      INFO : org.zerock.mapper.BoardMapperTests - delete count: 1
    ```
- `public int update(BoardVO board);`
  - 게시물의 업데이트는 제목, 내용, 작성자를 수정한다고 가정함.
  - 업데이트 할때는 최종 수정시간을 데이터베이스 내 현재 시간으로 수정함
  - Update는 delete와 마찬가지로 몇 개의 데이터가 수정되었는지 확인할 수 있기에
  - int 타입으로 메서드를 설계할 수 있음
