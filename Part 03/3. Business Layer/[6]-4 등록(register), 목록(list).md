### 1. 등록(Register) 작업의 구현과 테스트
- ##### 1. BoardServiceImpl
  - BoardSeviceImpl에서 파라미터로 전달되는 BoardVO 타입의 객체를 BoardMapper를 통해서 처리
  - BoardService는 void 타입으로 설계되어 있으므로 `mapper.insertSelectKey(board);`의 반환값인 int를 사용하지 않고 있지만 필요하다면 예외처리나 void 대신에 int 타입을 이용해서 사용할 수도 있음
  `public void register(BoardVO board);`
    > ```java
    > @Override
    > public void register(BoardVO board) {
    >   // TODO Auto-generated method stub
    >	  log.info("register:" + board);
    >	  mapper.insertSelectKey(board);
    > }
    >```

- ##### 2. BoardServiceTests
  - mapper의 insertSelectKey()를 이용해서 생성된 게시물의 번호를 확인할 수 있게 코딩함
    > ```java
    > @Test
    > public void testRegister() {
    >   BoardVO board = new BoardVO();
    >   board.setTitle("새로 작성하는 글");
    >   board.setContent("새로 직성하는 내용");
    >   board.setWriter("newbie");
    >   service.register(board);
    >   log.info("생성된 개시물의 번호: " + board.getBno());
    > }
    > ```
---
### 2. 목록(List) 작업의 구현과 테스트
- ##### 1. BoardSeviceImpl
  ```java
  @Override
  public List<BoardVO> getList() {
    // TODO Auto-generated method stub
    log.info("getList");
    return mapper.getList();
  }
  ```
- ##### 2. BoardServiceTests
  ```java
  @Test
  public void testGetList() {
    service.getList().forEach(board -> log.info(board));
  }
  ```
---
### 3. 조회(Read) 작업의 구현과 테스트
- ##### 1. BoardSeviceImpl
  ```java
  @Override
	public BoardVO get(Long bno) {
		log.info("get:" + bno);
		return mapper.read(bno);
	}
  ```
- ##### 2. BoardServiceTests
  ```java
	@Test
	public void testGet() {
		log.info(service.get(1L));
	}
  ```
---
### 4. 삭제, 수정 작업의 구현과 테스트
- ##### 1. BoardSeviceImpl
  - 삭제, 수정은 메서드의 리턴 타입을 void로 설계할 수도 있지만 엄격하게 처리하기 위해 boolean 타입으로 처리
  - == 연산자를 이용해 true/false 처리를 할 수 있음
  - testDelete의 경우 해당 개시물이 존재할 때 true를 반환
  - testUpdate는 먼저 특정한 게시물을 조회한 후 title 값 수정후 업데이트 함
  ```java
  @Override
	public boolean modify(BoardVO board) {
		log.info("modify: " + board);
		return mapper.update(board) == 1;
	}
	@Override
	public boolean remove(Long bno) {
		log.info("remove: " + bno);
		return mapper.delete(bno) == 1;
	}
  ```
- ##### 2. BoardServiceTests
  ```java
  @Test
  public void testDelete() {
		// 먼저 게시물 번호의 존재 여부를 확인후 테스트 진
		log.info("Remove Result: " + service.remove(2L));
  }

  @Test
  public void testUpdate() {
		BoardVO board = service.get(1L);
		if (board == null) {
			return;
		}
		board.setTitle("제목 수정합니다");
		log.info("Modify Result: " + service.modify(board));
  }
  ```
---
