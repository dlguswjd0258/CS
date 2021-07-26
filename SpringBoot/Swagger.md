# :female_detective: Swagger

## Swagger란?

- 대표적인 API 관리 도구
  - API 명세서 : 백 엔드 프로그램과 프론트 엔드 프로그램 사이에서 정확히 어떠한 방식으로 데이터 구조를 받을지에 대한 명세가 필요하고 이러한 내용이 담긴 문서

- 문서 자체에 api를 test 할 수 있다는 장점이 있다.



## JWT를 이용한 Authentication

- 문서에서 권한을 인증하고 test를 하기 위해서 필요한 설정

  ```java
  private ApiKey apiKey() {
      return new ApiKey(SECURITY_SCHEMA_NAME, "Authorization", "header");
  }
  
  private SecurityContext securityContext() {
      return SecurityContext.builder()
          .securityReferences(defaultAuth())
          .build();
  }
  
  public static final String SECURITY_SCHEMA_NAME = "JWT";
  public static final String AUTHORIZATION_SCOPE_GLOBAL = "global";
  public static final String AUTHORIZATION_SCOPE_GLOBAL_DESC = "accessEverything";
  
  private List<SecurityReference> defaultAuth() {
      AuthorizationScope authorizationScope = new AuthorizationScope(AUTHORIZATION_SCOPE_GLOBAL, AUTHORIZATION_SCOPE_GLOBAL_DESC);
      AuthorizationScope[] authorizationScopes = new AuthorizationScope[1];
      authorizationScopes[0] = authorizationScope;
      return newArrayList(new SecurityReference(SECURITY_SCHEMA_NAME, authorizationScopes));
  }
  ```




#### 궁금한 점

![image-20210716095757982](img\image-20210716095757982.PNG)

- value 값에 무엇을 넣어야 하는지
  - jwt secret 값? Bearer [jwt 토큰]



## 설정

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2).useDefaultResponseMessages(false)
                .select()
                .apis(RequestHandlerSelectors.any())
                .paths(PathSelectors.ant("/api/**")) // /api/모든 링크에 있는 Mapping
                .build()
                .securityContexts(newArrayList(securityContext()))
                .securitySchemes(newArrayList(apiKey()))
                ;
    }

    private ApiKey apiKey() {
        return new ApiKey(SECURITY_SCHEMA_NAME, "Authorization", "header");
    }

    private SecurityContext securityContext() {
        return SecurityContext.builder()
                .securityReferences(defaultAuth())
                .build();
    }

    public static final String SECURITY_SCHEMA_NAME = "JWT";
    public static final String AUTHORIZATION_SCOPE_GLOBAL = "global";
    public static final String AUTHORIZATION_SCOPE_GLOBAL_DESC = "accessEverything";

    private List<SecurityReference> defaultAuth() {
        AuthorizationScope authorizationScope = new AuthorizationScope(AUTHORIZATION_SCOPE_GLOBAL, AUTHORIZATION_SCOPE_GLOBAL_DESC);
        AuthorizationScope[] authorizationScopes = new AuthorizationScope[1];
        authorizationScopes[0] = authorizationScope;
        return newArrayList(new SecurityReference(SECURITY_SCHEMA_NAME, authorizationScopes));
    }

    @Bean
    UiConfiguration uiConfig() {
        return UiConfigurationBuilder.builder()
                .build();
    }
}
```



## 에러

### 1. Whitelabel Error Page

- 통합 빌드를 하고 swagger ui를 보려했는데 아래와 같응 에러 페이지가 나왔다

![image-20210712215011811](img\image-20210712215011811.PNG)

- 알고보니 3.0.0부터는 .html을 뺀 swagger-ui만 입력해야한다.

  `localhost:8080/swagger-ui`

