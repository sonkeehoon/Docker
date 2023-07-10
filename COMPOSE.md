![image](https://github.com/sonkeehoon/Docker/assets/81700507/794fec8f-f6f0-46a8-8b25-3afc1ecfa866)

# Docker Compose 
Docker의 기초를 공부한 뒤 추가적인 공부를 하는공간
- 공부 순서 : Docker 기초 - Docker compose(현재)
- 강의 자료 : https://www.youtube.com/watch?v=EK6iYRCIjYs
- 선수 과목
  - Docker 기초
    - https://github.com/sonkeehoon/Docker/blob/main/README.md
    - 참고영상 : https://www.youtube.com/playlist?list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf
- 실습 환경 : AWS EC2 인스턴스(ubuntu os)에 vscode remote ssh로 접속해서 docker를 설치했다
<hr><br>

## `Compose`
- container를 만드는 이런 명령이 있다고 가정해보자

![image](https://github.com/sonkeehoon/Docker/assets/81700507/8ecc8ba3-69cc-4ecd-bce5-c28ef8c88648)
- container를 만들때마다 이런 길고 복잡한 명령을 실행해야 된다면 굉장히 힘들어진다

![image](https://github.com/sonkeehoon/Docker/assets/81700507/40df4e1f-f4cd-4ad2-af24-eca656708eff)
- 이렇게 명령어를 .yml 형태로 스크립트로 보관하고
- 콘솔에서 docker-compose up 명령어만 실행하면 컨테이너가 실행된다면 얼마나 좋을까?

- 우리가 하려는것은 다음과 같다

![image](https://github.com/sonkeehoon/Docker/assets/81700507/08d0f521-5133-46ad-b05b-83016c784737)
- Web browser에서 Wordpress에 접속하면 Wordpress는 Mysql에 접속해서 작업을 한다
- 그 결과를 Wordpress가 가져와서 Web browser에게 보내준다
- 이러한 환경을 docker compose로 만들어보자
<br>

- 우분투 홈 디렉터리에 DOCKER-COMPOSE-SAMPLE라는 폴더를 만들고 폴더 안으로 들어가자
- 아래 링크를 띄워놓자(강의자료)
  - https://bit.ly/docker-compose-sample
  - command부분과 아래 docker-compose.yml은 같은 결과를 보여준다
- command부분을 일단 차례대로 실행해보자(터미널을 3개정도 분할해서 띄워놓자)
  - 1번 터미널에서 docker network create wordpress_net 실행하고
  - mysql 컨테이너를 생성하는 명령어도 실행하자
  - docker \
    run \
    --name "db" \
    -v "$(pwd)/db_data:/var/lib/mysql" \
    -e "MYSQL_ROOT_PASSWORD=123456" \
    -e "MYSQL_DATABASE=wordpress" \
    -e "MYSQL_USER=wordpress_user" \
    -e "MYSQL_PASSWORD=123456" \
    --network wordpress_net \
    mysql:5.7
  - 성공하면 db_data라는 폴더가 생성된다
  - 2번 터미널에서 wordpress를 실행하는 명령어를 실행하자
  - docker \
    run \
    --name app \
    -v "$(pwd)/app_data:/var/www/html" \
    -e "WORDPRESS_DB_HOST=db" \
    -e "WORDPRESS_DB_USER=wordpress_user" \
    -e "WORDPRESS_DB_NAME=wordpress" \
    -e "WORDPRESS_DB_PASSWORD=123456" \
    -e "WORDPRESS_DEBUG=1" \
    -p 8080:80 \
    --network wordpress_net \
    wordpress:latest
  - 성공하면 app_data라는 폴더가 생성된다
  - 잘 작동할까? 한번 접속을 해보자
    - http://3.39.137.68:8080/
    <br>
    
  ![image](https://github.com/sonkeehoon/Docker/assets/81700507/31ece19b-239e-4ce4-8a28-357050867d06)
  - 이런 화면이 뜨면 성공이다
    - wordpress를 설치하는 페이지라고 하는데, continue를 누르지는 않았다

- 이번엔 이렇게 복잡한 명령어를 기억할 필요없이 compose-up으로 같은결과를 내고싶다
- 방금 만든 컨테이너와 네트워크들을 모두 지우기위해 3개의 명령어를 실행하자
  - sudo docker rm -f app
  - sudo docker rm -f db
  - sudo docker network rm wordpress_net
- 컨테이너와 같이 host에 생성된 2개의 폴더도 지워주자
  - sudo rm -rf app_data db_data
- docker-compose.yml이라는 새 파일을 하나 만들자
  - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/2a025e4c-daa9-4664-ad32-2073d69db700)  
  - 아래 링크로 접속해서 docker-compose.yml부분을 복사해서 방금 만든 yml파일에 붙여넣자
    - https://bit.ly/docker-compose-sample
    - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/2d1fdcb9-d0be-46b5-a531-93b0728aa1ac)
  - 우선 docker-compose를 설치하자
    - sudo apt-get install docker-compose
  - 설치가 됐으면 실행하자
    - <ins>sudo docker-compose up</ins>
    - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/4864eecd-d8da-4edc-8bfb-1932b5f92f82)
    - 아까와 동일하게 2개의 폴더가 생성됐다
  - 설치가 잘 됐는지 확인해보자
    - http://3.39.137.68:8080/
    - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/984a085f-85f6-4203-858f-6be643aac931)
    - 아까와 같은 wordpress 설치안내 화면이 잘 뜬다
  - docker-compose로 실행한 컨테이너들을 끄고싶다면
    - sudo docker-compose down
    - 실행이 종료됨
- 우리가 했던것을 그림으로 나타내면 이러하다
- ![image](https://github.com/sonkeehoon/Docker/assets/81700507/efe1459e-2909-478b-9577-49844ab527a7)
- 글로 설명하기에는 복잡해서.. 아래 링크로 대체하려고 한다
  - https://youtu.be/EK6iYRCIjYs?t=372
  - shell 명령어와 yml파일 그리고 구체적인 그림들로 이해하기 쉽게 설명해주신다

<hr><br>

## `2023-07-07` docker compose 마무리
- docker compose란 명령어 입력 없이 yml파일에 명세된 대로 컨테이너들을 만들어주는 명령어다
- <strong>docker를 더 많이 공부해보고 싶다면</strong> https://seomal.com/map/1/129
<hr><br>
