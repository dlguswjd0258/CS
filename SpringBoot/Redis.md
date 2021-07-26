# :red_car:Redis (REmote DIctionary Server)

"키-값" 구조의 비정형 데이터를 저장하고 관리하기 위한 오픈소스 기반의 비관계형 데이터베이스 관리 시스템



## Redis 설치

### docker에 redis image 설치

- docer image

  -  서비스 운영에 필요한 서버 프로그램, 소스코드 및 라이브러리, 컴파일된 실행 파일을 묶은 형태

- 명령어

  `docker pull redis`



### 서버 구동



`docker run --name [container name] -p 6379:6379 -v [host와 연결한 폴더 경로] -d redis redis-server --appendonly yes`



