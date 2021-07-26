# :female_detective: Spring Boot gradle

## gradle

#### 정의

- "자동화 빌드 도구"로 프로그래밍 방식과 다양한 플러그인을 지원하는 하나의 빌드 플랫폼
- 소스 코드를 컴파일하고 jar나 war 형태 등으로 패키징해서 deploy하는 일을 자동화해주는 것



#### 특징

- java6 이상 사용 가능
- xml이 아닌 그루비로 작성되어 DSL(Domain-Specific-Laguage)을 스크림트로 사용
  - DSL : 특정 도메인에 특화된 프로그래밍 언어
- 하나의 Repository로 여러 프로젝트를 구성하는 멀티프로젝트 구성 가능
  - 상위 프로젝트의 의존성 및 설정을 하위 프로젝트에서 상속 받아 사용하는 것도 가능
- task를 만들거나 플러그인을 만들어 기능을 추가할 수 있어서 확장성이 좋음.



#### 터미널에서 빌드해서 실행하기

- gradle 명령어를 사용하기 위해 환경변수 설정

  - [gradle 설치파일 다운](https://gradle.org/releases/)

  - 적당한 위치에 압축 풀기

    - ex)C:\Gradle\gradle-7.1.1

  - 제어판 -> 시스템 -> 고급 시스템 설정 -> 환경변수 -> bin 경로 추가

    <img src="img\image-20210712174348146.PNG" alt="image-20210712174348146" style="zoom:67%;" />

- 빌드할 폴더에서 아래 명령어 실행

  ```
  gradle clean build
  ```



p.s gradle wrapper 사용하기

 - gradle 설치 파일 필요 없고 아래 명령어 사용

   ```
   gradlew clean build
   혹은
   gradlew.bat clean build
   ```

   

## build.gradle 설정 파일

- Maven 환경의 pom.xml과 같이 gradle 환경에서는 build.gradle 환경파일을 수정하여 관리



#### buildscript

- SpringBoot Version 정보, Maven Repository 정보, Dependency 모듈을 지정하여 스프링 부트 플러그인을 사용할 수 있는 기본 바탕을 정의

```gradle
buildscript{
    ext {
        springBootVer = '2.4.5'
        querydslVer = '4.4.0'
        querydslPluginVer = '1.0.10'
        springDependencyMgmtVer = '1.0.11'
        springLoadedVer = '1.2.8'
        nodePluginVer = '1.3.1'
    }
    repositories {
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath "org.springframework.boot:spring-boot-gradle-plugin:${springBootVer}"
        classpath "io.spring.gradle:dependency-management-plugin:${springDependencyMgmtVer}.RELEASE"
        classpath "org.springframework:springloaded:${springLoadedVer}.RELEASE"
        //이클립스인 경우를 위한 QueryDSL 플러그인. IntelliJ는 불필요. [시작]
        classpath "gradle.plugin.com.ewerk.gradle.plugins:querydsl-plugin:${querydslPluginVer}"
        //[끝] 
        classpath "com.github.node-gradle:gradle-node-plugin:3.1.0"
    }
}
```



#### apply

- 플러그인 적용

```gradle
apply plugin: 'io.spring.dependency-management'
apply plugin: 'eclipse'
apply plugin: 'com.github.node-gradle.node'
//이클립스인 경우를 위한 QueryDSL 플러그인. IntelliJ는 불필요.
apply plugin: 'com.ewerk.gradle.plugins.querydsl'
```



#### plugins

- buildscript와 apply 부분을 간단하게 적용

  ```gradle
  plugins {
      id 'java'
      id 'idea'
      id 'org.springframework.boot' version "${springBootVer}"
  }
  ```

  - version 뒤에 직접 `'2.4.5'` 작성하면 buildscript 필요 없음



#### dependencies

- 프로젝트에서 사용할 디펜던시 모듈 정의

  ```gradle
  dependencies {
      implementation("org.springframework.boot:spring-boot-starter-web")
      implementation("org.springframework.boot:spring-boot-starter-websocket")
      implementation("org.springframework.boot:spring-boot-starter-security")
      implementation("org.springframework.boot:spring-boot-starter-data-jpa")
      implementation("org.springframework.boot:spring-boot-starter-actuator")
      implementation("org.springframework.plugin:spring-plugin-core:2.0.0.RELEASE")
      testImplementation("org.springframework.security:spring-security-test")
      annotationProcessor("org.springframework.boot:spring-boot-starter-data-jpa")
      runtimeOnly("mysql:mysql-connector-java")
      developmentOnly("org.springframework.boot:spring-boot-devtools")
      annotationProcessor("org.springframework.boot:spring-boot-configuration-processor")
  
      implementation('commons-io:commons-io:2.6')
      implementation("org.apache.commons:commons-collections4:4.4")
      implementation("org.apache.commons:commons-lang3:3.9")
  
      implementation("com.querydsl:querydsl-jpa:${querydslVer}")
      implementation("com.querydsl:querydsl-apt:${querydslVer}")
  
      //STOMP 웹소캣 서버 사이드 테스트를 위한 의존성 추가
      implementation("org.springframework.boot:spring-boot-starter-mustache")
      //STOMP 관련 프론트 라이브러리
      implementation('org.webjars.bower:jquery:3.3.1')
      implementation('org.webjars:sockjs-client:1.1.2')
      implementation('org.webjars:stomp-websocket:2.3.3-1')
      implementation('org.webjars:webjars-locator:0.30')
      //WebRTC 클라이언트 의존성 추가
      implementation('org.webjars.bower:webrtc-adapter:7.4.0')
      //Kurento (미디어서버) 관련 의존성 추가
      implementation('org.kurento:kurento-client:6.16.0')
      implementation('org.kurento:kurento-utils-js:6.15.0')
  
  
      //IntelliJ용
      //IntelliJ에서는 하기 annotationProcessor를 쓰면 별도의 querydsl 플러그인 및 플러그인 설정이 불필요.
      //annotationProcessor("com.querydsl:querydsl-apt:${querydslVer}:jpa")
  
      implementation("com.squareup.retrofit2:retrofit:2.7.1")
      implementation("com.squareup.retrofit2:converter-jackson:2.7.1")
      implementation("com.squareup.okhttp3:logging-interceptor:3.9.0")
  
      implementation("com.google.guava:guava:29.0-jre")
      annotationProcessor("com.google.guava:guava:29.0-jre")
  
      testImplementation("com.jayway.jsonpath:json-path:2.4.0")
  
      implementation("com.auth0:java-jwt:3.10.3")
      
      implementation("io.springfox:springfox-swagger2:3.0.0")
      implementation("io.springfox:springfox-swagger-ui:3.0.0")
      implementation("io.springfox:springfox-data-rest:3.0.0")
      implementation("io.springfox:springfox-bean-validators:3.0.0")
      implementation("io.springfox:springfox-boot-starter:3.0.0")
  
      compile("javax.annotation:javax.annotation-api:1.2")
  
      implementation("org.projectlombok:lombok:1.18.20")
      annotationProcessor("org.projectlombok:lombok:1.18.20")
  
      testCompile('org.springframework.boot:spring-boot-starter-test')
  }
  ```

  - implementation, compile
    - 라이브러리 사용을 위한 컴파일을 해준다.
    - implementation **(권장)**
      - API 노출 X
      - A라를 모듈이 수정되어서 다시 빌드될 때 직접적으로 의존하는 모듈만 다시 빌드, 간접적으로 의존하는 건 X
    - compile **(비권장)**
      - API 노출 O
      - A라는 모듈이 수정되어서 다시 빌드될 때 직/간접적으로 의존하는 모듈 모두 다시 빌드



#### repositories

- 저장소 정보 관리

  ```gradle
  repositories {
      mavenCentral()
      maven { url 'https://repo.spring.io/snapshot' }
      maven { url 'https://repo.spring.io/milestone' }
      maven { url "https://repo.spring.io/libs-release" }
      maven { url "https://repo.maven.apache.org/maven2" }
      maven { url "https://build.shibboleth.net/nexus/content/repositories/releases" }
  }
  ```

  - mavenCentral() 메소드를 통해 중앙저장소 사용
  - 저장소는 각종 라이브러리 등이 등록된 일종의 소프트웨어 보관 장소
  - gradle이 저장소로부터 필요에 따라 프로그램을 다운로드하여 이용



## Maven보다 좋은 점

#### 1. 가독성

- 스크립트 길이와 가독성 면에서 Maven보다 뛰어나다.
- 설정 내용이 길어지면 xml은 가독성이 떨어짐



#### 2. 성능

- 빌드 및 테스트 시 성능이 뛰어나다.
- Gradle은 어떤 task가 업데이트 되었고 안되었는지 체크하기 때문에 incremental build를 허용
  - Incremental build : 반복적인 빌드 과정에서 변경된 소스코드에 의존성이 있는 대상들만 추려서 다시 빌드하는 기능
- 캐시를 사용하기 때문에 테스트 반복 시 차이가 크게 느껴진다.



##### 의존성이 늘어날 수록 성능과 스크립트 품질의 차이가 심해진다.

