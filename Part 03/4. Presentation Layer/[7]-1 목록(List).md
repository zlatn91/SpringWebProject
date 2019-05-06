### 1. 목록
1. Controller
- Service에 의존적이기 때문에 `@AllArgsConstructor`
를 이용해 생성자를 만들고 자동으로 주입하도록 함
- 만일 생성자를 만들지 않을 경우에는 `@Setter(onMethod_ = @Autowired)`를 이용함
- 리스트는 나중에 게시물의 목록을 전달해야 하므로 Model을 파라미터로 지정
- 이를 통해서 BoardServiceImpl 객체의 getList() 결과를 담아 전달 (addAttribute)
- BoardController의 테스트는 Spring의 테스트 기능을 통해서 확인해 볼 수 있음
```java
@Controller
@Log4j
@RequestMapping("/board/*")
@AllArgsConstructor
public class BoardController {

	private BoardService service;

	@GetMapping("/list")
	public void list (Model model) {
		log.info("list");
		model.addAttribute("list", service.getList());
	}
}

```
2. BoardControllerTests
- src/test/java -> org.zerock.controller
- 기존과는 좀 다르게 진행되는데 그 이유는 웹을 개발할 때 매번 URL을 테스트하기 위해서 Tomcat과 같은 WAS를
실행하는 불편한 단계를 생략하기 위해서
- WAS를 실행하지 않기 위해서는 약간의 코드가 필요하지만 반복적인 단계를 줄여줄 수 있기에 고려해 볼만한 방식
- 클래스 선언부의 @WebAppConfiguration 어노테이션을 적용
  - Servlet의 ServletContext를 이용하기 위해서인데 스프링에서는 `WebApplicationCotnext`라는 존재를 이용하기 위해서
- @Before 어노테이션이 적용된 setUp()에서는 import 할 때 JUnit을 이용해야함
  - 이가 적용된 메서드는 모든 테스트 전에 매번 실행되는 메서드가 됨
- MockMVC라는 말 그대로 '가짜 MVC'라고 생각하면 됨
  - 가짜 URL과 파라미터등을 브라우저에서 사용하는것처럼 만들어서 Controller의 역활을 실행해 볼 수 있음
- testList()는 MockMvcRequestBuilders 라는 존재를 이용해서 GET 방식의 호출을 함
 - 이후에는 BoardController의 getList()에서 반환된 결과를 이용해서 Model에 어떤 데이터들이 담겨 있는지 확인
 - Tomcat을 통해서 실행되는 방식이 아니므로 기존의 테스트 코드를 실행하는 것과 동일하게 실행함
  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  // Test for Contoller
  @WebAppConfiguration
  @ContextConfiguration({
  	"file:src/main/webapp/WEB-INF/spring/root-context.xml",
  	"file:src/main/webapp/WEB-INF/spring/appServlet/servlet-context.xml"})
  /*
   * Java Config
   * @ContextConfiguration(classes = {
   * 	org.zerock.config.RootConfig.class,
   * 	org.zerock.config.ServletConfig.class})
   */
  @Log4j
  public class BoardControllerTests {

  	@Setter(onMethod_ = @Autowired)
  	private WebApplicationContext ctx;

  	private MockMvc mockMvc;

  	@Before // import org.junit.Before;
  	public void setUp() {
  		this.mockMvc = MockMvcBuilders.webAppContextSetup(ctx).build();
  	}

  	@Test
  	public void testList() throws Exception{
  		log.info(
  				mockMvc.perform(MockMvcRequestBuilders.get("/board/list"))
  				.andReturn()
  				.getModelAndView()
  				.getModelMap());
  	}
  }
  ```
  - 아래의 결과가 보임
  ```
  INFO : org.zerock.controller.BoardControllerTests - {list=[BoardVO(bno=21, title=새로 작성하는 글, content=새로 작성하는 내용, writer=newbie, regdate=Wed Feb 06 04:09:36 KST 2019, updateDate=Wed Feb 06 04:09:36 KST 2019), ...
  INFO : org.springframework.web.context.support.GenericWebApplicationContext - Closing org.springframework.web.context.support.GenericWebApplicationContext@6e0e048a: startup date [Thu Feb 07 23:46:36 KST 2019]; root of context hierarchy
  INFO : com.zaxxer.hikari.HikariDataSource - HikariPool-1 - Shutdown initiated...
  ```
