# Chap1-1 REST API

### REST **API**

- REST
  - 인터넷 상에 있는 서로 다른 시스템 간의 독집적인 진화를 보장하는 방법
- REST 아키텍처 스타일을 따르는 API
- 오늘 날 REST API가 REST API가 아닌 이유
  - REST 아키텍처 스타일 중 Uniform Interface를 구성하는 self-descriptive message와 HETEOAS를 만족하지 않는다.





### Self-descriptive message

- 정의
  - 메시지 스스로가 메시지 본문에 대한 설명이 가능
  - 메시지 자체가 변하더라도 메시를 받는 클라이언트는 언제나 그 메시지 해석이 가능
  - 즉, 서버가 메시지를 바꾸더라도 클라이언트는 그 메시지를 보고 해석이 가능하다.

- 해결 방법
  방법1. 미디어 타입을 정의하고 IANA에 등록하고 그 미디어 타입을 리소스 리턴할 때 Content-	Type으로 사용

  방법2. profile 링크 헤더를 추가한다.

  - 브라우저들이 아직 스팩 지원을 잘 안함.
  - 대안으로 HAL의 링크 데이터에 profile 링크 추가




### HATEOAS

- 정의

  - 하이퍼미디어(링크)를 통해 애플리케이션 상태 변화가 가능
  - 링크 정보를 서버가 바꾸더라도 클라이언트에서 링크의 의미만 파악을 해서 이동이 가능
  - 즉, 링크 정보를 동적으로 바꿀 수 있다. (Versioning 할 필요 없이!)

- 해결 방법

  방법1. 데이터에 링크 제공	

  - 링크를 어떻게 정의 할 것인가? HAL

  방법2. 링크 헤더나 Location을 제공



### HAL (Hypertext Application Language)

- 링크 제공하는 방법 중 하나

- 이 강좌에서 위 두 가지를 만족시킬 방법으로 사용
  - 응답 본문에 profil 링크를 HAL 스펙에 따라 추가
