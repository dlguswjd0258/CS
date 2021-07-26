# :pencil: Chap2-1. Event API 클래스 생성

## TEST 만들기 (JUnit4 기준)

### 1. class 생성하기

- test/java에 ~.events package에 EventControllerTests class 생성

- Annotation 작성

  - @RunWith(SpringRunner.class)
  - @WebMvcTest
    - 웹과 관련된 Bean들 모두 등록 (슬라이스)
    - MockMvc 빈을 자동 설정

- MockMvc 사용하기

  ```java
  @Autowired
  MockMvc mockMvc;
  ```

  - 스프링 MVE 테스트 핵심 클래스
  - 웹 서버를 띄우지 않고도 스프링 MVC (DispatcherServlet)가 요청을 처리하는 과정을 확인할 수 있기 때문에 컨트롤러 테스트용으로 자주 쓰임.
  - Mocking 되어 있는 DispatcherServlet을 상대로 가짜 요청을 만들어서 보내고 응답을 테스트 할 수 있다.



### 2. 테스트 하기

```java
@Test
public void createEvent() throws Exception {
	mockMvc.perform(post("/api/events/")
				.contentType(MediaType.APPLICATION_JSON_UTF8)
				.accept(MediaTypes.HAL_JSON))
			.andExpect(status().isCreated()); // status().isCreated() = 201 응답
}
```

- post 요청이면 본문이 필요
  - contentType : 보내는 본문의 형식
  - accept : 원하는 응답 형식
- 위 코드는 404 에러가 난다. (다음 강의에서 마무리...)