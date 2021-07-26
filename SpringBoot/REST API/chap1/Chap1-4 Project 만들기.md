# Chap1-4 ~ 6 Project 만들기

## :one: Spring Boot project 생성

- JAVA 버전은 8이 깔려있어서 8로 사용했음.
- 추가한 의존성
  - Spring Web
  - Spring Data JPA
  - Spring HATEOAS
  - Spring REST Docs
  - H2 Database
    - 인메모리를 사용할 수 있는 DB 의존성
    - scope을 test로 했을 때 애플리케이션 실행 시에 사용되지 않음
    - 대신 postgresql이 실행 됨.
    - scope이 test가 아니고 postgresql 둘 두 있을 경우 test에 어떤 설정도 없다면 h2가 실행 된다.
  - PostgreSQL Driver
    - 인메모리가 아닌 DB 의존성
  - Lombok
    - Lombok 사용시 다운 받고 설정 필요
- @EnableAutoConfiguration : 자동 설정



#### :diamond_shape_with_a_dot_inside: Lombok 설치 및 설정 :diamond_shape_with_a_dot_inside:

1. 설치

   ![image-20210707200422548](img/image-20210707200422548.png)

   - 위 사이트에서 lombok.jar 파일 설치

2. STS.exe 위치에 lombok.jar 파일 옮기기

   ![image-20210707200939347](img\image-20210707200939347.png)

3. lombok.jar 실행

   - jar 파일을 압축 파일로 인식해 .zip 처럼 열린다면 아래와 같이 실행한다.

     1. JDK 혹은 zulu 폴더에 bin 폴더 경로를 복사한다.

        <img src="img\image-20210707201445869.png" alt="image-20210707201445869" style="zoom: 67%;" />

     2. cmd 창을 열어 복사한 경로로 간다.
     
     3. 아래와 같이 명령어를 입력한다.
     
        ```
        java -jar lombok.jar파일경로
        ```
     
     ![image-20210707202105901](img\image-20210707202105901.png)

4. Specify lovation... -> STS.exe 선택 -> STS 체크 -> Install / Update -> Quit Installer

   ![image-20210707202231085](img\image-20210707202231085.png)

   ![image-20210707202315042](img\image-20210707202315042.png)

   ![image-20210707202431057](\img\image-20210707202431057.png)
   
   

## :two: Event 도메인 구현

- #### Event Class 생성

  ```java
  @Builder @AllArgsConstructor @NoArgsConstructor
  @Getter @Setter @EqualsAndHashCode(of = "id")
  public class Event {
  	private Integer id;
  	private String name;
  	private String description;
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
  	private EventStatus eventStatus;
  }
  ```

  - @Builder
    - 장점 : 입력하는 값이 무엇인지 알 수 있다.
    - builder는 public이 아니다보니 다른 package에서 사용할 수가 없으므로 생성자를 사용해야하지만, builder가 생성되면 기본 생성자 생성 안되므로 생성자 사용 불가
    - 그러므로, @AllArgsConstructor @NoArgsConstructor 필요
  - @AllArgsConstructor
    - 모든 인자를 담는 생성자
  - @NoArgsConstructor
    - 기본 생성자
  - @EqualsAndHashCode(of = "id")
    - 객체 간의 상호 참조하는 연관 관계라면 Equals와 HashCode를 구현한 코드 안에서 스택 오버플로우가 발생할 수 있으므로 id로만 Equals와 HashCode를 비교하도록 사용
    - 즉, Equals와 HashCode를 id만 사용하라는 의미  

  

  :exclamation: p.s. Spring이 제공해주는 Annotation은 Custom Annotation 만들 수 있자먼 lombok은 불가능

  :exclamation: p.s. @Data 사용은 @EqualsAndHashCode 때문에 비권장

  

- #### Test class 만들기

  - test/java에 Event와 같은 package로 EventTest class 생성

  - builder 유무 확인

    ```java
    @Test
    public void builder() {
        Event event = Event.builder()
    			.name("Inflearn Spring REST API")
    			.description("REST API development with Spring")
    			.build();
    	assertThat(event);
    }
    
    @Test
    public void javaBean() {
    	// Given
    	String name = "Event";
    	String description = "Spring";
    
    	// When
    	Event event = new Event();
    	event.setName(name);
    	event.setDescription(description);
    
    	// Then
    	assertThat(event.getName()).isEqualTo(name);
    	assertThat(event.getDescription()).isEqualTo(description);
    }
    ```



### :rotating_light:  오류 해결

- no tests found with test runner 'JUnit 5' 오류

  ![image-20210707222319417](\img\image-20210707222319417.png)

  - JUnit 4로 테스트하는데 설정이 JUnit 5로 되어있을 때 생기는 오류

  - 해결방법
    - run configurations -> JUnit -> Test class 선택 -> Test runner 설정 -> JUnit 4로 변경

      ![image-20210707223433110](\img\image-20210707223433110.png)

  



## :three: Event 비즈니스 로직 : Event 생성 API 구현

### 입력값

| 변수명                  | 설명                                  |
| ----------------------- | ------------------------------------- |
| name                    | 이벤트 이름                           |
| description             | 이벤트 설명                           |
| beginEnrollmentDateTime | 이벤트 등록 시작 시간                 |
| closeEnrollmentDateTime | 이벤트 등록 종료 시간                 |
| beginEventDateTime      | 이벤트 시작 일시                      |
| endEventDateTime        | 이벤트 종료 일시                      |
| location (optional)     | 이벤트 위치 (이게 없으면 온라인 모임) |
| basePrice (optional)    | 등록비                                |
| maxPrice (optional)     | 등록비                                |
| limitOfEnrollment       | 등록 제한 인원수                      |

- basePrice와 maxPrice

  | basePrice | maxPrice | 설명                                                         |
  | --------- | -------- | ------------------------------------------------------------ |
  | 0         | 100      | 선착순 등록                                                  |
  | 0         | 0        | 무료                                                         |
  | 100       | 0        | 무제한 경매 (높은 금액 낸 사람이 등록)                       |
  | 100       | 200      | 제한가 선착순 등록<br>처음부터 200을 낸 사람은 선 등록.<br>100을 내고 등록할 수 있으나 더 많이 낸 사람에 의해 밀려날 수 있음. |



### 결과값

- id 
- name 
- ... 
- **eventStatus: DRAFT**, PUBLISHED, ENROLLMENT_STARTED, ...
- offline
-  free
- _links 
  - profile (for the self-descriptive message)
  - self 
  - publish 
  - ... 

