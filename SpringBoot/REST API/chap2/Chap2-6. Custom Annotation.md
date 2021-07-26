# :pencil: Chap2-6. Annotation 만들기

### 1. TestDescription Annotation 만들기

- TestDescription을 interface로 만들고 interface 앞에 @ 붙이면 된다.

  ```java
  public @interface TestDescription {
  }
  ```

- Customize하기 위한 Annotation 붙이기

  ```java
  @Target(ElementType.METHOD)
  @Retention(RetentionPolicy.SOURCE)
  ```

  - Target
    - 어노테이션이 적용할 위치 선택
    - ElementType.PACKAGE : 패키지 선언
    - ElementType.TYPE : 타입 선언
    - ElementType.ANNOTATION_TYPE : 어노테이션 타입 선언
    - ElementType.CONSTRUCTOR : 생성자 선언
    - ElementType.FIELD : 멤버 변수 선언
    - ElementType.LOCAL_VARIABLE : 지역 변수 선언
    - ElementType.METHOD : 메서드 선언
    - ElementType.PARAMETER : 전달인자 선언
    - ElementType.TYPE_PARAMETER : 전달인자 타입 선언
    - ElementType.TYPE_USE : 타입 선언
  - Retention
    - 자바 컴파일러가 어노테이션을 다루는 방법을 기술하며, 특정 시점까지 영향을 미치는지 결정
    - RetentionPolicy.SOURCE : 컴파일 전까지만 유효. (컴파일 이후에는 사라짐)
    - RetentionPolicy.CLASS : 컴파일러가 클래스를 참조할 때까지 유효.
    - RetentionPolicy.RUNTIME : 컴파일 이후에도 JVM에 의해 계속 참조가 가능. (리플렉션 사용)



### 2. 최종 코드

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface TestDescription {
	String value();
}
```

