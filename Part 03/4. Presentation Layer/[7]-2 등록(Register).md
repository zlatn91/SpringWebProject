### 1. 등록
> ##### 1. Controller
- Post 방식으로 처리되는 register()를 작성
- String을 리턴 타입으로 지정
- RedirectAttributes 를 파라미터로 지정
  - 이는 등록 작업이 끝난 후 다시 목록 화면으로 이동하기 위함
  - 추가적으로 새롭게 등록된 게시물의 번호를 같이 전달하기 위해서 RedirectAttributes를 이용함
- 리턴시에는 'redirect:' 접두어를 사용하는데 이를 이용하면 스프링 MVC가 내부적으로 response.sendRedirect()를 처리해 주기 때문에 편리함

```java
@Controller
@Log4j
@RequestMapping("/board/*")
@AllArgsConstructor
public class BoardController {

	private BoardService service;

	@PostMapping("/register")
	public String register(BoardVO board, RedirectAttributes rttr) {
		log.info("register: " + board);
		service.register(board);
		rttr.addFlashAttribute("result", board.getBno());
		return "redirect:/board/list";
	}
}
```
> ##### 2. Test
- 테스트 할때 MockMvcRequestBuilders의 post()를 이용하면 POST 방식으로 데이터를 전달할 수 있음
- parma()을 이용해서 전달해야 하는 파라미터들을 지정할 수 있음
  - (input 태그와 유사한 역할)
  - 이러한 방식으로 코드를 작성하면 최초 작성시에는 일이 많다고 느껴지지만 매번 입력할 필요가 없기 때문에
  - 오류가 발생하거나 수정하는 경우 반복적인 테스트가 수월해 짐
```java
@Test
public void testRegister() throws Exception {
  String resultPage = mockMvc.perform(MockMvcRequestBuilders.post("/board/register")
      .param("title", "테스트 새 글 제목")
      .param("content", "테스트 새 글 내")
      .param("writer", "user00")
      ).andReturn().getModelAndView().getViewName();

  log.info(resultPage);
}
```
- 테스트 결과 상단에 BoardVO 객체로 올바르게 데이터가 바인딩된 결과를 볼 수 있음
- 중간에는 SQL의 실행 결과가 보이고 마지막에는 최종 문자열을 반환함
```
INFO : jdbc.audit - 1. Connection.prepareStatement(insert into tbl_board (bno, title, content, writer)
		values (?, ?, ?, ?)) returned net.sf.log4jdbc.sql.jdbcapi.PreparedStatementSpy@5e63cad
INFO : jdbc.audit - 1. PreparedStatement.setLong(1, 43) returned
INFO : jdbc.audit - 1. PreparedStatement.setString(2, "테스트 새 글 제목") returned
INFO : jdbc.audit - 1. PreparedStatement.setString(3, "테스트 새 글 내") returned
INFO : jdbc.audit - 1. PreparedStatement.setString(4, "user00") returned
INFO : jdbc.sqlonly - insert into tbl_board (bno, title, content, writer) values (43, '테스트 새 글 제목', '테스트 새 글 내',
'user00')

INFO : jdbc.sqltiming - insert into tbl_board (bno, title, content, writer) values (43, '테스트 새 글 제목', '테스트 새 글 내',
'user00')
 {executed in 8 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
INFO : jdbc.audit - 1. PreparedStatement.isClosed() returned false
INFO : jdbc.audit - 1. PreparedStatement.close() returned
INFO : jdbc.audit - 1. Connection.clearWarnings() returned
INFO : org.zerock.controller.BoardControllerTests - redirect:/board/list
```
---
