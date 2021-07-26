# Chap1-2 Event REST API

### 이벤트 목록 조회 - [GET] /api/events

- 로그인 안 한 상태
  - 응답에 보여줘야 할 데이터
    - 이벤트 목록
    - 링크
      - self
      - profile : 이벤트 목록 조회 API (문서로 링크)
      - get-an-event : 이벤트 하나 조회하는 API 링크
      - next (optional) : 다음 페이지
      - prev (optional) : 이전 페이지
  - 문서 => 스프링 REST Docs로 만들 예정
- 로그인 한 상태
  - 응답에 보여줘야 할 데이터
    - 이벤트 목록
    - 링크
      - self
      - profile : 이벤트 목록 조회 API (문서로 링크)
      - get-an-event : 이벤트 하나 조회하는 API 링크
      - create-new-envet : 이벤트를 생성할 수 있는 API 링크 => 이거 하나 추가
      - next (optional) : 다음 페이지
      - prev (optional) : 이전 페이지

p.s. 로그인 상태 여부는 AccessToken으로 판단

### 이벤트 등록 - [POST] /api/events



### 이벤트 하나 조회 - [GET] /api/events/{id}



### 이벤트 수정 - [PUT] /api/events/{id}



