# :video_camera:WebRTC​

WebRTC란?

- P2P 통신 프로토콜
- RTC 기능을 제공하는 프로토코과 APIs
- 소프트웨어나 별도의 플로그인이 필요없고 Peer와 Peer 간의 P2P 연결을 통해 video, data 스트리밍이 가능
- 



STUN 서버를 통해서 자신의 Public IP를 알아내어 그 정보를 통해서 시그널링을 함.

(라우터 테이블에 자신의 private IP가 맴핑되어 있다.)

TUTN 서버 

- 모든 데이터를 교환할때에는 특정 서버를 거쳐서 그 서버에 저장된 테이블을 통해 보내주는 방식

- STUN 서버를 포함하고 있는 superset(TURN ⊇ STUN)의 개념
- 네트워크 리소스 비용 많이 발생
- 퍼블릭 IP만 알려주는 STUN과 달리 모든 데이터가 거쳐가기 때문



WebRTC는 HTTPS를 반드시 통해야한다.
