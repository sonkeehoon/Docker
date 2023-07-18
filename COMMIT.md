![image](https://github.com/sonkeehoon/Docker/assets/81700507/9a971556-226a-46c3-abb1-9dcf65733b26)


# Docker Commit (공부중)
Docker의 기초를 공부한 뒤 추가적인 공부를 하는공간
- 공부 순서 : Docker 기초 - Docker compose - Docker commit(현재)
- 강의 자료 : https://www.youtube.com/watch?v=RMNOQXs-f68
- 선수 과목
  - Docker 기초
    - https://github.com/sonkeehoon/Docker/blob/main/README.md
    - 참고영상 : https://www.youtube.com/playlist?list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf
- 실습 환경 : AWS EC2 인스턴스(ubuntu os), vscode remote ssh, docker
<hr><br>

## `Commit`
- 도커의 image를 만드는 명령어가 commit
- 도커의 동작 과정
  - docker hub에서 우리가 원하는 image를 다운로드(pull)
  - image를 run해서 컨테이너 생성
- 직접만든 image를 만들어서 배포해보면 어떨까? 하는 생각
- 내가 수정한 컨테이너를 다시 image로 바꾸는 명령이 바로 commit
- 쉽게말해 run의 정반대라고 생각하면 된다
<hr><br>

## `2023-07-07` docker commit 마무리
- docker commit은 image를 만드는 명령어
- <strong>docker를 더 많이 공부해보고 싶다면</strong> https://seomal.com/map/1/129
<hr><br>
