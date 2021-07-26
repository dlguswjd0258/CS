# 🤜Github 사용법🤛

### 1. Github 시작하기

1) github에 repository 만들기

![image](https://user-images.githubusercontent.com/62821450/112742240-b998e100-8fc7-11eb-907f-f2dc544b0188.png)



### 2. 로컬 저장소와 연결하기

- 처음 repository를 생성하면 아래와 같이 뜬다.
- 여기에 나온 명령어를 따라하면된다. (나는 ...or push ~를 사용함.)
- 아래에 좀 더 자세히...

![image](https://user-images.githubusercontent.com/62821450/112742641-26fa4100-8fcb-11eb-8ab0-c6e9d43f96d4.png)



1) 로컬에서 프로젝트를 관리할 로컬 디렉토리 저장소를 생성

![image](https://user-images.githubusercontent.com/62821450/112742691-86585100-8fcb-11eb-8ddd-cb7bec3bf73b.png)



2) git remote add [단축이름] [url] 하기

- 로컬 디렉토리 저장소에서 git bash를 연다.
- git remote add를 한고 main branch로 이동한다.

![image](https://user-images.githubusercontent.com/62821450/112744140-451a6e00-8fd8-11eb-9b4e-e0541125867b.png)

- 위의 명령은 origin이라는 이름으로 리모트 저장소가 등록되었다는 의미.
- git remote -v를 입력하면 단축이름 origin과 원격 저장소의 url을 확인할 수 있다.

p.s) git remote remove origin을 하면 origin이라는 이름을 가진 리모트 저장소를 삭제하는 것.



3) git push -u [원격저장소별칭] [현재 브랜치 이름]

- 마지막으로 로컬 저장소에 있는 파일을 u 옵션을 통해 원격저장소로 push.

![image](https://user-images.githubusercontent.com/62821450/112744730-91b47800-8fdd-11eb-9e03-f0246444a406.png)

* error: src refspec master does not match any 에러가 난 이유는 pull 없이 push할 경우 기존 내용을 삭제하거나 하는 문제가 생길 수 있기 때문에, 이런 문제를 피하고자 에러 메세지를 발생 시킨 것.
* 그래서 이런 에러메세지가 났다면 pull을 한 후에 push를 해준다.

![image](https://user-images.githubusercontent.com/62821450/112744722-7fd2d500-8fdd-11eb-83a2-1fda03a7f198.png)



### 3. branch

- branch를 각자 사용하는 이유는 작업을 분리하기 위해서이다.
- 작업을 분리해서 작업을 한 뒤에 main branch에 merge하면 된다.

| 명령어                       | 설명                              |
| ---------------------------- | --------------------------------- |
| git branch [브랜치이름]      | branch 생성                       |
| git checkout [브랜치이름]    | branch 이동                       |
| git checkout -b [브랜치이름] | branch 생성 후 생성 branch로 이동 |
| git branch                   | branch 리스트 확인                |
| git status                   | 내가 작업하고 있는 branch 확인    |
| git branch -m [브랜치이름]   | 브랜치 이름 변경                  |



### 4. push

`git add [. 또는 해당파일이름]`

`git commit -m "message"`

`git push`



### 5. pull

