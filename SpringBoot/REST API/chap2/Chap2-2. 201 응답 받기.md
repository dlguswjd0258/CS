# :pencil: Chap2-2. 201 응답 받기

### 1. Controller class 생성

- 이전 강의에서 테스트하려던 createEvent는 Controller가 없어서 에러가 났던 것이기 때문에 Controller 생성하기

  ```java
  @Controller
  public class EventController {
  
  	@PostMapping("/api/events")
  	public ResponseEntity createEvent() {
          // /api/events/{id}
  		URI createdUri = linkTo(methodOn(EventController.class).createEvent()).slash("{id}").toUri();
  		return ResponseEntity.created(createdUri).build();
  	}
  }
  ```

  - ResponseEntity를 사용하는 이유
    - 응답 코드, 헤더, 본문 모두 다루기 편한 API
  - linkTo
    - HATEOAS에서 제공하는 메서드
    - Controller.class를 넣으면 Controller에 적용된 RequestMapping()이 들어간다.
    - 하나의 메서드만 넣고싶다면 methodOn 사용
  - methodOn
    - HATEOAS에서 제공하는 메서드
    - 해당 클래스의 메서드를 참조하는 데 사용

  

  p.s. linkTo와 methodOn은 org.springframework.hateoas.server.mvc.WebMvcLinkBuilder 패키지에 있다.



### 2. ​​ 테스트 하기

- 전 강의에서 작성한 테스트 실행하면 성공적으로 실행된다.

- JSON 응답으로 201이 나오는지 확인

  - Location 헤더에 생성된 이벤트를 조회할 수 있는 URI가 담겨 있는지 확인.

    ```java
    .andDo(print()).andExpect(~);
    ```

  - id를 잘 받는지 확인.

    ```java
    // 아이디가 있는지 확인
    .andExpect(jsonPath("id").exists())
    ```

    - 위 코드를 추가하면 id라는 응답이 없어서 test가 깨지기 때문에 제대로된 요청을 만들어서 test해야한다.

- 본문 추가하기

  1. Event 생성하기

     ```java
     Event event = Event.builder()
     				.name("Spring")
     				.description("REST API development with Spring")
     				.beginEnrollmentDateTime(LocalDateTime.of(2021, 07,08,14,21))
     				.closeEnrollmentDateTime(LocalDateTime.of(2021, 07,09,14,21))
     				.beginEventDateTime(LocalDateTime.of(2021, 07,10,14,21))
     				.endEventDateTime(LocalDateTime.of(2021, 07,11,14,21))
     				.basePrice(100)
     				.maxPrice(200)
     				.limitOfEnrollment(100)
     				.location("강남역 D2 스타텁 팩토리")
     				.build();
     ```

     

  2. json으로 본문 넣어주기

     - ObjectMapper 사용하기
       - Spring Boot를 사용할 때 mapping jackson json이 의존성으로 들어가 있다면 자동으로 Bean으로 등록 해준다.

     ```java
     mockMvc.perform(post("/api/events/")
              .contentType(MediaType.APPLICATION_JSON_UTF8)// 본문을 json으로 보내겠다는 의미
              .accept(MediaTypes.HAL_JSON) // 응답 형식
              .content(objectMapper.writeValueAsString(event))) // 본문
         ~
     ```

     - postMapping이기 때문에 Controller에서 본문을 받는 인자가 필요

       - methodOn으로 하니까 createEvent()에 null값을 매개변수로 넘겨줘야하는데 그렇게 하지 않기 위해 아래와 같이 변경

       ```java
       @Controller
       @RequestMapping(value = "/api/events", produces = MediaTypes.HAL_JSON_VALUE)
       public class EventController {
       
       	@PostMapping
       	public ResponseEntity createEvent(@RequestBody Event event) {
       		URI createdUri = linkTo(EventController.class).slash("{id}").toUri();
       		event.setId(10);
       		return ResponseEntity.created(createdUri).body(event);
       	}
       	
       }
       ```

       

