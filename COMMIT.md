![image](https://github.com/sonkeehoon/Docker/assets/81700507/9a971556-226a-46c3-abb1-9dcf65733b26)


# Docker Commit
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
- 도커의 image를 만드는 명령어를 commit이라고 한다

![image](https://github.com/sonkeehoon/Docker/assets/81700507/3dada664-f255-467a-9ba6-299b82b7dcbc)

- 도커의 동작 과정
  - docker hub에서 우리가 원하는 image를 다운로드(pull)
  - image를 run해서 컨테이너 생성
- 직접만든 image를 만들어서 배포해보면 어떨까? 하는 생각
- 내가 수정한 컨테이너를 다시 image로 바꾸는 명령이 바로 commit
- 쉽게말해 run의 정반대라고 생각하면 된다
- push는 이렇게 만든 image를 docker hub같은 레지스트리에 공개적으로 업로드하는 과정
<br>

이런 실험을 해보자<br>
![image](https://github.com/sonkeehoon/Docker/assets/81700507/ea51b237-07a4-4ba7-aeeb-336e800353df)
- 우리가 docker hub에서 가져온 어떤 컨테이너에 git을 설치했다고 가정하자
  - 영상에선 ubuntu 이미지를 다운받았지만 나는 이미 ubuntu에서 작업중이기 때문에 centos7 image를 다운받았다
- git을 설치한 이 컨테이너를 새로운 이미지로 만들자(앞으로 만들 컨테이너에 공통적으로 git을 설치해두기 위해)
- 이 새로운 이미지로 여러 컨테이너들을 만들어보자(각각 PHP, Python, Nodejs등이 깔린)
<br>

- centos가 깔린 docker image부터 다운받자
  - sudo docker pull centos:7
- image 다운 성공
  - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/ff4b630e-997d-4a9a-aea8-abbc80fc5857)
- sudo docker run -it --name my-centos7 centos:7 bash
  - -it : 만들자 마자 컨테이너 내부 터미널로 이동
  - --name my-centos7 : my-centos라는 이름의 컨테이너
  - 위에서 다운받은 centos 이미지
  - bash : bash쉘에서 실행
- 컨테이너가 잘 생성됐는지 확인하려면 다른 터미널에서 sudo docker ps 
  - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/50b46c68-7033-401d-bd19-7004e99cea63)
  - 생성 완료
- 컨테이너 내부에 업데이트 및 git 설치
  - yum update
    - 우분투에서 apt update는 금방 끝나는데 yum update는 좀 시간이 걸린다
  - yum install git
- 만들어진(git을 추가로 설치한)컨테이너를 이미지로 만들어보자
- sudo docker commit my-centos7 keehoon:centos7-git
  - my-centos7 : commit 당할 컨테이너 이름
  - keehoon : respository 이름
  - centos7-git : 이미지 이름
- 만들어 졌는지 확인
- sudo docker images
  - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/1ad29bf0-4bf8-42e3-a009-1f3ca616f46e)
  - 생성 완료
- 잘 만들어진 이미지를 써먹어보자
- 두개의 터미널을 키고 각각의 터미널에서 컨테이너를 만들자
- 1번 터미널
  - sudo docker run -it --name nodejs keehoon:centos7-git bash
  - yum update
  - yum install epel-release
  - yum install nodejs
- 2번 터미널
  - sudo docker run -it --name python keehoon:centos7-git bash
  - yum update && yum install python3
<br>

- 확인 해보자
- 1번 터미널
  - node -v
    - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/fb3becde-0252-4bca-a02d-d696693f2f27)
- 2번 터미널
  - python --version
    - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/5191be35-75c7-4cd8-a710-b89ba2302044)
  - 2번에서 node -v를 해도 실행되지 않는다
    - 1번 터미널에서 실행한 컨테이너와 독립적인 환경이기 때문에
<br>

- (추가)이 모든 과정을 Dockerfile 하나로 만들어 놓을수도 있다
- Dockerfile을 만든후 다음과 같이 입력하고
  - FROM centos:7
  - RUN yum -y update && yum install -y git
  - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/2c2f5b63-7262-46e8-a865-357e65670c4d)
- 다음 명령을 실행하면 된다
- sudo docker build -t keehoon:centos-git-2 .
  - build : image를 만들어라
  - -t keehoon:centos-get-2 : 만들 이미지의 (레포지토리:태그명)을 정하는 t옵션
  - . : 현재 위치(.)의 Dockerfile로
- 이렇게 하면 위의 commit과정과 같은 결과를 얻는다

<hr><br>

## `2023-07-18` docker commit 마무리
- docker commit은 컨테이너를 image로 만드는 명령어
- docker build 또한 image를 만드는 명령어 이지만
  - commit과 다른점은 Dockerfile을 이용해서 만든다는 점 (컨테이너 없이도 가능)
- <strong>docker를 더 많이 공부해보고 싶다면</strong> https://seomal.com/map/1/140
<hr><br>
