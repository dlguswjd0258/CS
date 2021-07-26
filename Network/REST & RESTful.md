# :fist_right: REST & RESTful :fist_left:

### 1. REST란 (Representational State Transfer)

자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미.

자원은 넒은 의미로 해당 소프트웨어가 관리하는 모든것이 될 수 있다. 문서, 그림, 데이터 등.

예를 들어, DB에 학생 정보가 저장되어 있다고 한다면 이 학생들의 정보가 자원이 된다.

  

일반적으로 REST라고 하면 좁은 의미로 HTTP를 통해 CRUD를 실행하는 API를 뜻한다.

HTTP 프로토콜을 이용하기 때문에 URI(route) 통해 자원을 표시하고 <a href="https://blog.naver.com/PostView.nhn?blogId=adlguswjda&logNo=222290747964&categoryNo=80&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postList&userTopListOpen=true&userTopListCount=5&userTopListManageOpen=false&userTopListCurrentPage=1">HTTP methods</a>를 통해 할 일(CRUD)을 처리하도록 설계된 아키텍처이다.

![image](https://user-images.githubusercontent.com/62821450/112750125-9559f600-9001-11eb-90da-70fb5889d4dc.png)



### 2. REST의 구성 요소

1. 자원(Resource) : URI

   - 모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
   - 자원을 구별하는 ID는 '/students/:gtudent_id'와 같은 **HTTP URI**다.

     

2. 행위(Verb) : HTTP METHOD

   - HTTP 프로토콜을 Method를 사용한다.
   - HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메서드를 제공한다.

     

3. 표현(Representations)

   - Client가 자원의 상태(정보) 에 대한 조작을 요청하면 Server는 이에 적절한 응답(Representation)을 보낸다.
   - 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation으로 나타내어 질 수 있고, 일반적으로 JSON, XML을 통해 데이터를 주고 받는다.

   

### 3. REST 특징

1. Server-Client (서버-클라이언트 구조)
2. Stateless(무상태)
   - Client의 context를 Server에 저장하지 않는다. 즉, 세션과 쿠키와 같은 context 정보를 신경쓰지 않아도 되므로 구현이 단순.
   - Server는 각각의 요청을 완전히 별개의 것으로 인식하고 처리. 즉, 이전 요청이 다음 요청의 처리에 연관되어서는 안된다.
3. Cacheable(캐시 처리 가능)
   - 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
4. Layered System(계층화)
5. Code-On-Demand(optional)
6. Uniform Interface(인터페이스 일관성)
   - HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용 가능. 즉, 특정 언어나 기술에 종속 X

   

---------------

### 1. RESTful이란

일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어로 REST API'를 제공하는 웹 서비스를 'RESTful'하다고 할 수 있다.

RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것이 아니라 개발자마다 생각하는 RESTful의 내용이 다르다. 

RESTful의 목적은 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것이다.

   

### 2. RESTful 7가지 routes 설정법

| **URL**          | **HTTP Verb** | **Action** | Description                  |
| ---------------- | ------------- | ---------- | ---------------------------- |
| /photos/         | GET           | index      | 모든 photos의 목록 표시      |
| /photos/new      | GET           | new        | 새로운 photos 만들 양식 표시 |
| /photos          | POST          | create     | photos 생성                  |
| /photos/:id      | GET           | show       | 하나의 photos 정보 표시      |
| /photos/:id/edit | GET           | edit       | 하나의 photos 수정 양식 표시 |
| /photos/:id      | PATCH/PUT     | update     | 하나의 photos 수정           |
| /photos/:id      | DELETE        | destroy    | 하나의 photos 삭제           |

> resource는 영어 복수형으로 적는다. ex) photos
>
> :id는 하나의 특정한 resource를 나타낼 수 있는 고유의 값이다.

   

### 3. RESTful하지 못한 예

- CRUD기능을 모두 POST로만 처리하는 API
- Route에 resource, id 외 정보가 들어가는 경우