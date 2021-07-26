# :pencil: Chap2-7. Bad Request 응답

### 1. 본문에 메시지 만들기

- error 정보들은 전부 errors에 배열로 담겨 있다
  - field : 어떤 필드에서 에러가 났는지
  - rejectedValue : 해당 필드에서 에러를 발생시킨 값
  - objectName : 객체 이름
  - codes : 에러 코드
  - defaultMessage : 에러 메시지
- error 종류
  - FieldError
  - GloblaError

- body에 errors를 담아서 넘겨주는 것이 안됨
  - Errors를 그냥 Json으로 변환할 수 없기 때문에
  - Event는 되고 Errors는 안되는 이유
    - Event는 Java Baen 스펙을 준수하고 있기 때문에 기본으로 등록되어 있는 BeanSerializer 사용해서 변환 가능하지만 Errors는 Java Bean 스펙을 준수하고 있지 않다
    -  따라서, Custom Serializer를 만들어야 한다.



#### :pencil: ErrorsSerializer 만들기

- FieldError와 GlobalError(ObjectError) 둘다 만들어 줘야한다.
  - GlobalError에는 field와 rejectedValue 없음
- Errors가 Serializer할 때 사용할 수 있도록 @JsonComponent로 ObjectMapper에 등록하기

```java
@JsonComponent // ObjectMapper에 등록하기
public class ErrorsSerializer extends JsonSerializer<Errors> {
	@Override
	public void serialize(Errors errors, JsonGenerator gen, SerializerProvider serializers) throws IOException {
		// errors 안에는 error가 여러개니까 배열로 담아주기 위해서 StartArray와 EndArray를 사용
		gen.writeStartArray();
		
		// FieldError 담기
		errors.getFieldErrors().stream().forEach(e -> {
			try {

				// JSON Object 채우기
				gen.writeStartObject();
				gen.writeStringField("field", e.getField());
				gen.writeStringField("objectName", e.getObjectName());
				gen.writeStringField("code", e.getCode());
				gen.writeStringField("defaultMessage", e.getDefaultMessage());

				// RejectedValue는 있을 수도 있고 없을 수도 있다.
				Object rejectedValue = e.getRejectedValue();
				if (rejectedValue != null) {
					gen.writeStringField("rejectedValue", rejectedValue.toString());
				}
				
				gen.writeEndObject();
			} catch (IOException e1) {
				e1.printStackTrace();
			}
		});
		
		// GlobalError 담기
		errors.getGlobalErrors().stream().forEach(e -> {
			try {
				// JSON Object 채우기
				gen.writeStartObject();
				gen.writeStringField("objectName", e.getObjectName());
				gen.writeStringField("code", e.getCode());
				gen.writeStringField("defaultMessage", e.getDefaultMessage());

				gen.writeEndObject();
			} catch (IOException e1) {
				e1.printStackTrace();
			}
			
		});;
		gen.writeEndArray();
	}
}
```







### 2. TEST 하기

```java
@Test
@TestDescription("입력 값이 잘못된 경우 에러가 발생하는 테스트")
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
			.andDo(print())
			.andExpect(status().isBadRequest())
			.andExpect(jsonPath("$[0].objectName").exists()) // 에러배열에서 객체이름 확인
			//.andExpect(jsonPath("$[0].field").exists()) // 어떤 field에서 발생했는지
			.andExpect(jsonPath("$[0].defaultMessage").exists()) // 기본 메시지는 무엇인지
			.andExpect(jsonPath("$[0].code").exists()) // 에러 코드가 무엇인지
			//.andExpect(jsonPath("$[0].rejectedValue").exists()) // 에러가 발생된 값이 무엇인지
			;
}
```

- field와 rejectedValue가 없는 GloblaError일 경우 에러가 나기 때문에 시간관계상 제거하고 test

