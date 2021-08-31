# ArrayList vs LinkedList

## ArrayList

​	![image](https://user-images.githubusercontent.com/62821450/131496981-14a00956-4aa8-4f5c-a797-c28fb7273bdc.png)

- 중복 허용
- 순서 유지
- 인덱스로 원소 관리
- 배열 형식을 취하지만 크기가 고정되지 않음
- 연산 속도가 O(n)으로 자료의 최대 개수에 영향 받음



#### 삽입

![image](https://user-images.githubusercontent.com/62821450/131500255-91923436-8fe2-4886-9c3e-fa7346083775.png)

1. 삽입할 자료만큼 공간을 늘린다
2. 삽입할 자료의 위치를 기준으로 기존의 데이터들을 앞이나 뒤로 이동하는 연산을 수행
3. 삽입할 위치에 자료가 입력되면 삽입 연산을 마친다



#### 삭제

![image](https://user-images.githubusercontent.com/62821450/131501669-1e223377-b8fb-4b27-b5a9-d7a3d0efdf4f.png)

1. 삭제될 자료가 위치한 인덱스의 자료를 삭제
2. 삭제한 자료의 인덱스를 기준으로 이후의 자료들을 이동하는 연산을 수행
3. List의 맨 마지막은 비어있는 상태로 완료한다.



## LinkedList

​	![image](https://user-images.githubusercontent.com/62821450/131497235-b7daa73c-fff9-4288-b6ad-712156ec7a14.png)

- 자료의 주소값으로 서로 연결되어 있는 구조
- 내부적으로 양방향의 연결 리스트로 구성
- 순방향 또는 역순으로 순회 가능
- 새로운 자료의 삽입이나 기존 자료의 삭제를 위치 관계없이 빠른 시간안에 수행 가능





#### 삽입

![image](https://user-images.githubusercontent.com/62821450/131508257-21495cd4-6da2-4d2e-a9ee-91b82e6658b2.png)

1. 추가될 자료의 node 생성
2. 추가될 자료의 인덱스 이전 node와 추가될 자료 인덱스 이후 node를 설정



#### 삭제

![image](https://user-images.githubusercontent.com/62821450/131508405-62877e4f-7049-4d5b-bac9-ea81a7221e53.png)

1. 삭제할 노드의 이전 노드와 이후 노드를 연결

