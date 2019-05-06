### 1. 수정
- 등록 작업과 유사함
- 변경된 내용을 수집해서 BoardVO 파라미터로 처리
- BoardService를 호출함
- 수정을 시작하는 화면의 경우에는 GET 방식을 접근하지만
- 실제 작업은 POST 방식으로 동작하므로 @PostMapping
> 1. controller
- service.modify()는 수정 여부를 boolean으로 처리하믈 이를 이용해서 성공한 경우에만 RedirectAttributes에 추가

  ```java
  @PostMapping("/modify")
  public String modify(BoardVO board, RedirectAttributes rttr) {
    log.info("modify: " + board);
    if(service.modify(board)) {
      rttr.addFlashAttribute("result", "success");
    }
    return "redirect:/board/list";
  }
  ```
> 2. test
```java
@Test
public void testModify() throws Exception{
  String resultPage = mockMvc.perform(MockMvcRequestBuilders.post("/board/modify")
      .param("bno", "1")
      .param("title", "수정된 테스트 새글 제목")
      .param("content", "수정된 테스트 새글 내용")
      .param("writer", "user00"))

      .andReturn().getModelAndView().getViewName();
  log.info(resultPage);
}
```
---
2. 삭제
- 조회와 유사하게 작성
- 삭제는 반드시 POST 방식으로만 처리
> 1. controller
   -  삭제 후 페이지의 이동이 필요하므로 RedirectAttributes 를 파라미터로 사용
   - redirect:/ 를 이용해서 삭제처리 후에 다시 목록 페이지로 이동
  ```java
  @PostMapping("/remove")
  public String remove(@RequestParam("bno") Long bno, RedirectAttributes rttr) {
    log.info("remove bno: " + bno);
    if(service.remove(bno)) {
      rttr.addFlashAttribute("result", "success");
    }
    return "redirect:/board/list";
  }
  ```
> 2. test
  - MockMVC를 이용해서 파라미터를 전달할 때에는 문자열로만 처리해야 함
  ```java
  @Test
  public void testRemove() throws Exception{

    String resultPage =
        mockMvc.perform(MockMvcRequestBuilders
        .post("/board/remove")
        .param("bno", "21"))

        .andReturn().getModelAndView().getViewName();

    log.info(resultPage);
  }
  ```
> 3. console
```
INFO : jdbc.audit - 1. Connection.prepareStatement(delete tbl_board where bno = ?) returned net.sf.log4jdbc.sql.jdbcapi.PreparedStatementSpy@2d0566ba
INFO : jdbc.audit - 1. PreparedStatement.setLong(1, 21) returned
INFO : jdbc.sqlonly - delete tbl_board where bno = 21

INFO : jdbc.sqltiming - delete tbl_board where bno = 21
 {executed in 6 msec}
INFO : jdbc.audit - 1. PreparedStatement.execute() returned false
INFO : jdbc.audit - 1. PreparedStatement.getUpdateCount() returned 1
```a
