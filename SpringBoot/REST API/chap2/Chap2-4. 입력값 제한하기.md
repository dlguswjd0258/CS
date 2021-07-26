# :pencil: Chap2-4. 입력값 제한하기

### 1. 제한되어야 하는 입력값

- id
  - db에 들어갈 때 자동 생성될 값
- free
  - price에 값이 있을 경우에는 false
- offline
  - location에 값이 있을 때는 true
- eventStatus



### 2. DTO로 제한하기

- jackson json이 제공하는 Annotation으로도 제한 할 수 있다.
  - 하지만, 도메인(Entity) 클래스에서 우려하는 점은 Annotation이 너무 많아지는 것
  - 너무 많은 Annotation이 생기면 어떤 용도의 Annotation인지 기억이 안날 수 있다.
- 입력 값을 받는 용으로 DTO 클래스를 생성
  - 직접 받을 입력들만 속성으로 남겨 놓는다.
- 단점은 중복된 속성이 있다.



###### :pencil: Controller EventDto로 인자 변경

```java
@PostMapping
public ResponseEntity createEvent(@RequestBody EventDto eventDto) {
    Event event = modelMapper.map(eventDto, Event.class);
    Event newEvent = this.eventRepository.save(event); // 객체 저장 후 저장된 객체 가져오기
    URI createdUri = linkTo(EventController.class).slash(newEvent.getId()).toUri();
    return ResponseEntity.created(createdUri).body(event);
}
```

- ModelMapper를 이용해 Event 타입으로 변경해 EventRepository 사용

  - 원래는 아래처럼 다 셋팅을 해야하지만 ModelMapper를 통해서 생략 가능

    ```java
    Event event = Event.builder()
    			.name(eventDto.getName())
    			.description(eventDto.getDescription())
    			.build();
    ```

  - 물론, 리플렉션이 일어나지만 자바 버전이 올라갈수록 개선되고 있기 때문에 크게 우려될 정도는 아니다.

  - maven 등록 필요

    ```xml
    <!-- https://mvnrepository.com/artifact/org.modelmapper/modelmapper -->
    <dependency>
        <groupId>org.modelmapper</groupId>
        <artifactId>modelmapper</artifactId>
        <version>2.3.8</version>
    </dependency>
    ```

  - 공용으로 사용 할 수 있는 객체이기 때문에 Bean으로 등록해서 사용

    ```java
    @SpringBootApplication
    public class RestApiWithSpringApplication {
    
    	public static void main(String[] args) {
    		SpringApplication.run(RestApiWithSpringApplication.class, args);
    	}
    	
    	@Bean
    	public ModelMapper modelMapper() {
    		return new ModelMapper();
    	}
    }
    ```

    

###### :ballot_box_with_check: Entity 클래스와 DTO 클래스를 분리하는 이유

- View Layer와 DB Layer의 역할을 철저하게 분리하기 위해서
  - 
- 테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되는 반면 View와 통신하는 DTO 클래스(Request / Resopse 클래스)는 자주 변경되므로 분리해야 함.
- Domain Model을 아무리 잘 설계했다고 해도 각 View 내에서 Domain Model의 getter만을 이용해서 원하는 정보를 표시하기가 어려운 경우가 종종 있다. 이런 경우 Domain Model 내에 Presentation을 위한 필드나 로직을 추가하게 되는데, 이러한 방식이 모델링의 순수성을 깨고 Domain Model 객체를 망가뜨리게 된다.
- 또한, Domain Model을 복잡하게 조합한 형태의 Presentation 요구사항들이 있기 때문에 Domain Model을 직접 사용하는 것은 어렵다.
- 즉, DTO는 Domain Model을 복사한 형태로, 다양한 Presentation Logic을 추가한 정도로 사용하며 Domain Model 객체는 Persistent만을 위해서 사용한다.



### 3. 테스트 하기

- 이전 코드로 실해하게 되면 nullpointer 에러 발생

  - controller가 save한 객체는 새로 만든 객체이기 때문에 실제로는 test에서의 객체와 controller에서 save한 객체가 다르다. 그렇기 때문에 Mocking이 적용 안되서 null return이 됨.
  - Event로 Mocking을 했더라 하더라도 Controller에서 EventDto로 받았기 때문에 id가 넘어가지 않는다.
  - 그래서 Mocking하지 않고 데이터를 실제로 저장을 해서 테스트를 한다면 더 이상 Slicing Test가 아니여야한다.

- 코드

  ```java
  @RunWith(SpringRunner.class)
  @SpringBootTest // 기본값 Mock : Mocking을 한 DispatcherServlet을 만들도록
  @AutoConfigureMockMvc
  public class EventControllerTests {
  
  	@Autowired
  	MockMvc mockMvc;
  
  	@Autowired
  	ObjectMapper objectMapper;
  		
  	@Test
  	public void createEvent() throws Exception {
  		Event event = Event.builder()
  						.id(100)
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
  						.free(true)
  						.offline(false)
  						.build();
  		
  		mockMvc.perform(post("/api/events/")
  					.contentType(MediaType.APPLICATION_JSON_UTF8)// 본문에 json을 보내고 있다.
  					.accept(MediaTypes.HAL_JSON) // 원하는 응답 형식
  					.content(objectMapper.writeValueAsString(event))) // 본문
  				.andDo(print()) // 실제 응답값을 보고싶을 떼
  				.andExpect(status().isCreated()) // 201 응답 예상
  				.andExpect(jsonPath("id").exists()) // 아이디가 있는지 확인 
  				.andExpect(header().exists(HttpHeaders.LOCATION)) // Location 헤더가 있는지 확인
  				.andExpect(header().string(HttpHeaders.CONTENT_TYPE, MediaTypes.HAL_JSON_VALUE)) // 해당 헤더에 특정한 값이 나오는지 확인
  				.andExpect(jsonPath("id").value(Matchers.not(100))) 
  				.andExpect(jsonPath("free").value(Matchers.not(true))) 
  				;
  	}
  }
  ```

  - SpringBootTest에서 MockMvc를 쓰려면 @AutoConfigureMockMvc 필요



##### error 해결

```
java.lang.SecurityException: class "org.hamcrest.Matchers"'s signer information does not match signer information of other classes in the same package
```

- JUnit4에는 org.hamcrest package에 Matchers라는 class가 없기때문에 org.hamcrest.core package에 있는 IsNot 클래스에 있는 not method 사용



```
java.lang.AssertionError: JSON path "eventStatus" expected:<DRAFT> but was:<null>
```

- Event 클래스의 eventStatus 기본값을 DRAFT로 설정

  ```java
  @Enumerated(EnumType.STRING)
  private EventStatus eventStatus = EventStatus.DRAFT;
  ```

  
