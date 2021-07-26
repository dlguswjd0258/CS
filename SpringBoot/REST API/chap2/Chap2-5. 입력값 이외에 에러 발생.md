# :pencil: Chap2-5. 입력값 이외에 에러 발생

#### :star: 이전 강의와 다른 방법:star:

### 1. BadReqeust 값 받기

- properties 설정하기

  - spring boot가 제공하는 ObjectMapper 확장 기능 사용하기

    ```properties
    #unknown properties가 있으면 실패하라
    spring.jackson.deserialization.fail-on-unknown-properties=true
    ```

  - deserialization : json 문자열을 object로 변환하는 과정

  - serialization : object를 json으로 변환하는 과정

- 이전 강의처럼 입력값 제한 줄 때 무시하는 방법은 사용자에게 오해를 줄 수있다.
  - 예를 들어, 무시되는 값을 입력해도 201 응답을 받으니까 값을 받는다는 인식을 줄 수있다.

### 2. Test 코드

```java
@Test
public void createEvent_Bad_Request() throws Exception {
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
			.eventStatus(EventStatus.PUBLISHED)
			.build();
	
	mockMvc.perform(post("/api/events/")
			.contentType(MediaType.APPLICATION_JSON_UTF8)// 본문에 json을 보내고 있다.
			.accept(MediaTypes.HAL_JSON) // 원하는 응답 형식
			.content(objectMapper.writeValueAsString(event))) // 본문
			.andDo(print()) // 실제 응답값을 보고싶을 떼
			.andExpect(status().isBadRequest()) // 400 응답 예상
	;
}
```

- id, free, offline, eventStatus는 unknown properties이기 때문에 BadRequest를 받는다.
