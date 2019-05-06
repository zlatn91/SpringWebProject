### 1. 조회
- 특별한 경우가 아니면 조회는 GET 방식으로 처리함
> 1. controller
- @GetMapping
- get() 메서드에서는 bno 값을 좀 더 명시적으로 처리하는 RequestParam을 이용해서 지정
   - 파라미터 이름과 변수 이름을 기준으로 동작하기 때문에 생략해도 무방
- 화면 쪽으로 해당 번호의 게시물을 전달해야 하므로 Model을 파라미터로 지정
  ```java
  @GetMapping("/get")
  public void get(@RequestParam("bno") Long bno, Model model) {
    log.info("/get");
    model.addAttribute("board", service.get(bno));
  }
  ```
> 2. test
-  특정 게시물을 조회할때는 반드시 bno라는 파라미터가 필요하므로 param()을 통해서 추가하고 실행함
  ```java
  @Test
  	public void testGet() throws Exception{
  		log.info(
  				mockMvc.perform(MockMvcRequestBuilders
  				.get("/board/get")
  				.param("bno", "43"))

  				.andReturn().getModelAndView().getModelMap());
  	}
  ```
> 3. log
  - 파라미터가 제대로 수집되었는지 확인
  - Model에 담겨 있는 BoardVO 인스턴스의 내용을 살펴볼 수 있음
  ```
  INFO : org.zerock.controller.BoardControllerTests - {board=BoardVO(bno=43, title=테스트 새 글 제목, content=테스트 새 글 내, writer=user00, regdate=Thu Feb 07 15:01:25 KST 2019, updateDate=Thu Feb 07 15:01:25 KST 2019), org.springframework.validation.BindingResult.board=org.springframework.validation.BeanPropertyBindingResult: 0 errors}
INFO : org.springframework.web.context.support.GenericWebApplicationContext - Closing org.springframework.web.context.support.GenericWebApplicationContext@6e0e048a: startup date [Fri Feb 08 11:36:19 KST 2019]; root of context hierarchy
INFO : com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Shutdown initiated...
  ```
---
