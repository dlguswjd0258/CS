# :pencil:JWT 프로젝트 생성 및 설정

## 프로젝트 생성

1. dependency 추가

   ![image-20210721162338067](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20210721162338067.png)

   - jwt를 위한 dependency는 https://mvnrepository.com/artifact/com.auth0/java-jwt/3.18.0 여기에서 가져와서 build.gradle에 추가

     ```gradle
     implementation 'com.auth0:java-jwt:3.18.0'
     ```

     

2. SecurityConfig 설정

   ```java
   @Configuration
   @EnableWebSecurity
   @RequiredArgsConstructor
   public class SecurityConfig extends WebSecurityConfigurerAdapter{
   	
   	private final CorsFilter corsFilter;
   
   	@Override
   	protected void configure(HttpSecurity http) throws Exception {
   		http.csrf()
   			.and()
   			.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS) // 세션을 사용하지 않겠다는 의미
   			.and()
   			.addFilter(corsFilter) 
   			.formLogin().disable() // jwt서버이기 때문에 id, password를 formLogin을 하지 않음
   			.httpBasic().disable() // 기본적인 http 방식 사용하지 않음
   			.authorizeRequests()
   			.antMatchers("/api/v1/user/**")
   			.access("hasRole('ROLE_USER') or hasRole('ROLE_ADMIN') or hasRole('ROLE_MANAGER')")
   			.antMatchers("/api/v1/manager/**")
   			.access("hasRole('ROLE_MANAGER') or hasRole('ROLE_ADMIN')")
   			.antMatchers("/api/v1/admin/**")
   			.access("hasRole('ROLE_ADMIN')")
   			;
   	}
   }
   ```

   - addFilter(corsFilter)
     - Filter를 통해 Cors 설정
     - 인증이 없을 땐 @CrossOrigin 사용해도 되지만 인증이 필요한 페이지는 필터에 등록해줘야 한다.

3. 

