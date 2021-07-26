# :pencil: Chap2-3. Event Repository

### 1. Event에 Annotation 추가

```java
@Builder @AllArgsConstructor @NoArgsConstructor
@Getter @Setter @EqualsAndHashCode(of = "id")
@Entity
public class Event {
	
	@Id @GeneratedValue
	private Integer id;
	private String name;
	private String description;
	
	// localDateTime 자동 mapping
	private LocalDateTime beginEnrollmentDateTime;
	private LocalDateTime closeEnrollmentDateTime;
	private LocalDateTime beginEventDateTime;
	private LocalDateTime endEventDateTime;
	private String location; // (optional) 이게 없으면 온라인 모임
	private int basePrice; // (optional)
	private int maxPrice; // (optional)
	private int limitOfEnrollment;
	private boolean offline;
	private boolean free;
	@Enumerated(EnumType.STRING)
	private EventStatus eventStatus;
	
}
```

**@Entity** 

- 객체와 테이블 매핑
- JPA가 관리

**@Id** 

- 기본키(PK) 설정

**@GeneratedValue**

- 기본 키 자동 생성
- GenerationType.IDENTITY 
  - 기본 키 생성을 데이터베이스에 위임 (= AUTO_INCREMENT)
  - MySQL, SQL Server, DB2
- GenerationType.SEQUENCE 
  - 데이터베이스 시퀀스를 사용해서 기본 키 할당
  - 즉, 시퀀스 오브젝트를 만들고 이걸 통해 id 값을 가져옴.
  - @SequenceGenerator 필요
  - 오라클, PostgreSQL, DB2, H2
- GenerationType.TABLE
  - 키 생성용 테이블을 만들어서 데이터베이스 시퀀스를 흉내내는 전략
  - 모든 데이터베이스에 적용 가능
  - 단, 성능이 좋지 않다.
- GenerationType.AUTO (기본값)
  - 선택한 데이터베이스 방언에 따라 방식을 자동으로 선택

**@Enumerated(EnumType.STRING)**

- EnumType.ORDINAL(기본값) : enum 순서 값을 DB에 저장
  - enum의 순서가 바뀌면 데이터가 꼬일 수 있음.
- EnumType.STRING : enum 이름을 DB에 저장



### 2. Repository 생성 후 주입

- EventRepository Interface를 생성 후 JpaRepository<Event, Integer>  상속 받기

  ```java
  public interface EventRepository extends JpaRepository<Event, Integer> {
  }
  ```

- Controller에 Repository 주입

  ```java
  public class EventController {
  
  	@Autowired
  	EventRepository eventRepository;
  	...
  }
  ```

  



### 3. 테스트 하기

- @WebMvcTest가 슬라이스 test이기 때문에 Web용 Bean들만 등록을 해주므로, Repository를 Mocking해야한다.

  ```java
  @MockBean
  EventRepository eventRepository;
  ```

- Mock 객체는 save나 무엇을 하더라도 return 값이 null이기 때문에 NullPointer 에러가 나므로 test 코드에 아래 코드 추가한다.

  ```java
  @Test
  public void createEvent() throws Exception {
  	...
  	event.setId(10);
  	Mockito.when(eventRepository.save(event)).thenReturn(event);
  	...
  }
  ```

- header test하기

  ```java
  .andExpect(header().exists(HttpHeaders.LOCATION)) // Location 헤더가 있는지 확인
  .andExpect(header().string(HttpHeaders.CONTENT_TYPE, MediaTypes.HAL_JSON_VALUE)) // 해당 헤더에 특정한 값이 나오는지 확인
  ```

  

### :o: EventController Test 전체 코드

```java
@RunWith(SpringRunner.class)
@WebMvcTest
public class EventControllerTests {

	@Autowired
	MockMvc mockMvc;

	@Autowired
	ObjectMapper objectMapper;
	
	@MockBean
	EventRepository eventRepository;
	
	@Test
	public void createEvent() throws Exception {
		Event event = Event.builder()
						.name("Spring")
						.description("REST API development with Spring")
						.beginEnrollmentDateTime(LocalDateTime.of(2021, 07, 8, 14, 21))
						.closeEnrollmentDateTime(LocalDateTime.of(2021, 07, 9, 14, 21))
						.beginEventDateTime(LocalDateTime.of(2021, 07, 10, 14, 21))
						.endEventDateTime(LocalDateTime.of(2021, 07, 11, 14, 21))
						.basePrice(100)
						.maxPrice(200)
						.limitOfEnrollment(100)
						.location("강남역 D2 스타텁 팩토리")
						.build();
		
		// Mock 객체인 eventRepository를 불러서 nullpointer 에러가 났을 때 아래와 같은 코드 작성
		event.setId(10);
		Mockito.when(eventRepository.save(event)).thenReturn(event);
		
		mockMvc.perform(post("/api/events/")
					.contentType(MediaType.APPLICATION_JSON_UTF8)// 본문에 json을 보내고 있다.
					.accept(MediaTypes.HAL_JSON) // 원하는 응답 형식
					.content(objectMapper.writeValueAsString(event))) // 본문
				.andDo(print()) // 실제 응답값을 보고싶을 떼
				.andExpect(status().isCreated()) // 201 응답 예상
				.andExpect(jsonPath("id").exists()) // 아이디가 있는지 확인 
				.andExpect(header().exists(HttpHeaders.LOCATION)) // Location 헤더가 있는지 확인
				.andExpect(header().string(HttpHeaders.CONTENT_TYPE, MediaTypes.HAL_JSON_VALUE)) // 해당 헤더에 특정한 값이 나오는지 확인
				;
	}
}
```























