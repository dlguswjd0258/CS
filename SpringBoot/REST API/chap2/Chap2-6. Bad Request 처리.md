# :pencil: Chap2-6. Bad Request 처리

### 1. 입력값이 잘못되었을 때 처리하기

#### :one: 필수로 입력해야 하는 값이 없을 때

- 이름이 없거나 날짜가 없을 때

- @Vaild

  - dependency 추가

    ```xml
    <dependency> 
        <groupId>org.springframework.boot</groupId> 
        <artifactId>spring-boot-starter-validation</artifactId> 
    </dependency>
    ```

  - Request에 들어있는 값들을 바인딩할 때 검증 가능

- 필수인 속성에 Annotation 붙여주기

  - @NotEmpty : String
  - @NotNull : 날짜
  - @Min(값) : int

- @Vaild 사용한 인자 바로 옆에 Errors 받기

  ```java
  @PostMapping
  public ResponseEntity createEvent(@RequestBody  @Valid EventDto eventDto, Errors errors) {
      ...
  }
  ```



#### :two:입력 값이 이상할 때

- 시작 날짜가 끝 날짜보다 빠르거나 기본 가격이 max 가격보다 클 때 등

- Annotation으로 검증하기 어려운 경우이기 대문에 직접 Vaildator를 만든다.

  ```java
  @Component // Bean 등록
  public class EventValidator {
  	public void vaildate(EventDto eventDto, Errors errors) {
  
  		// 무제한 경매는 max값이 0이기 때문에 이런 경우가 아니면 base값이 작다
  		if (eventDto.getBasePrice() > eventDto.getMaxPrice() && eventDto.getMaxPrice() != 0) {
  			// basePrice가 이상하다
  			errors.rejectValue("basePrice", "wrongValue", "BasePrice is Wrong");
  			// maxPrice가 이상하다
  			errors.rejectValue("maxPrice", "wrongValue", "MaxPrice is Wrong");
  		}
  
  		LocalDateTime endEventDateTime = eventDto.getEndEventDateTime();
  		if (endEventDateTime.isBefore(eventDto.getBeginEventDateTime()) ||
  			endEventDateTime.isBefore(eventDto.getCloseEnrollmentDateTime()) ||
  			endEventDateTime.isBefore(eventDto.getBeginEnrollmentDateTime())) {
  			// 종료 날짜가 이상할 때
  			errors.rejectValue("endEventDateTime", "wrongValue", "endEventDateTime is Wrong");
  		}
  
  		// TODO beginEventDateTime
  		// TODO CloseEnrollmentDateTime
  		}
  	}
  }
  ```

  - Controller에 주입해서 사용



#### :three: BadRequest 값 return하기

```java
@Autowired
EventValidator eventValidator;

@PostMapping
public ResponseEntity createEvent(@RequestBody  @Valid EventDto eventDto, Errors errors) {
	if(errors.hasErrors()) {
		return ResponseEntity.badRequest().build();
	}
	
	eventValidator.vaildate(eventDto, errors);
	if(errors.hasErrors()) {
		return ResponseEntity.badRequest().build();
	}
	
    ...
}
```



### 2. TEST 하기

```java
@Test
public void creatEvent_Bad_Request_Empty_Input() throws Exception {
	EventDto eventDto = EventDto.builder().build();
	
	this.mockMvc.perform(post("/api/events")
					.contentType(MediaType.APPLICATION_JSON_UTF8)// 본문에 json을 보내고 있다.
					.content(objectMapper.writeValueAsString(eventDto))) // 원하는 응답 형식
				.andExpect(status().isBadRequest());
}

@Test
public void creatEvent_Bad_Request_Wrong_Input() throws Exception {
	EventDto eventDto = EventDto.builder()
						.name("Spring")
						.description("REST API development with Spring")
						.beginEnrollmentDateTime(LocalDateTime.of(2021, 07, 9, 14, 21))
						.closeEnrollmentDateTime(LocalDateTime.of(2021, 07, 8, 14, 21))
						.beginEventDateTime(LocalDateTime.of(2021, 07, 11, 14, 21))
						.endEventDateTime(LocalDateTime.of(2021, 07, 10, 14, 21))
						.basePrice(10000)
						.maxPrice(200)
						.limitOfEnrollment(100)
						.location("강남역 D2 스타텁 팩토리")
						.build();
	
	this.mockMvc.perform(post("/api/events")
			.contentType(MediaType.APPLICATION_JSON_UTF8)// 본문에 json을 보내고 있다.
			.content(objectMapper.writeValueAsString(eventDto))) // 원하는 응답 형식
	.andExpect(status().isBadRequest());
}
```

