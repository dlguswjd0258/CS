# :video_camera:kurento

kurento란?

- WebRTC 미디어 서버이고 www 및 스마트 폰 플랫폼 용 클라이언트 API 세트

- 미디어 서버
  - P2P 방식에서 중간에 중계 역할을 하는 서버
  - SFU 방식
    - 단순히 받은 데이터를 연결도니 Peer들에게 뿌려줌
    - 중간 처리 없이 그대로 전달
    - 
  - MCU 방식 => 쿠렌토 방식
    - 중안에서 비디오를 인코딩 등과 같은 전처리를 하여 Peer에게 전달
    - 중간에서 믹싱
    - 인코딩을 통해서 압축률을 좋게 하여 각 Peer들에게 던져주며 네트워크 리소스 비용에서 유리하나 중앙에서 처리해주는 서버의 CPU 리소스를 많이 잡아먹는다
- JAVA로 된 API를 지원하며 레퍼런스가 비교적 잘 되어 있다.
- "무료"이다
- 컴퓨터 비전, 비디오 인덱싱, 증강 현실 및 음성 분석과 관련된 고급 미디어 처리 기능도 제공
- 개발자는 SDK를 사용하여 쿠렌토를 조작할 수 잇게 간단하게 구현



미디어 서버를 왜 사용하는가

- P2P 방식은 1:N, N:M은 메시 구조를 가져야하기 때문에 중간에 중계를 해주는 미디어 서버가 필요



WebRTC 미디어 서버

- 미디어 트래픽이 통과하는 멀티미디어 미들웨어
- 들어오는 미디어 스트림을 처리하고 다른 결과물을 내보낼 수 있다.
- 예를 들어
  1. Group Communications : 한 명의 스트리머가 방송을 시작하고, 그 방에 접속해 있는 참여자들에게 미디어 스트림을 배포 (다중 회의 장치 MCU 역할 수행)
  2. Mixing : 여러 수신 스트림을 하나의 복합 스트림으로 변환
  3. Transcoding : 호환되지 않는 클라이언트 간에 코덱 및 형식을 즉성에서 조정
  4. Recording : 미디어에 들어오는 스트림을 영구적인 방식으로 저장
  5. 
