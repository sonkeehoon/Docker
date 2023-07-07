![image](https://github.com/sonkeehoon/Docker/assets/81700507/794fec8f-f6f0-46a8-8b25-3afc1ecfa866)

# Docker Compose 
Docker의 기초를 공부한 뒤 추가적인 공부를 하는공간
- 강의 자료 : https://www.youtube.com/watch?v=EK6iYRCIjYs
- 선수 과목 : Docker 기초
  - https://github.com/sonkeehoon/Docker/blob/main/README.md
  - 참고영상 : https://www.youtube.com/playlist?list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf 
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
- 우선 방금 만든 컨테이너와 네트워크들을 모두 지우기위해 3개의 명령어를 실행하자
  - sudo docker rm -f app
  - sudo docker rm -f db
  - sudo docker network rm wordpress_net
- 컨테이너와 같이 생성된 2개의 폴더도 지워주자
  - sudo rm -rf app_data db_data
- docker-compose.yml이라는 새 파일을 하나 만들자
  - ![image](https://github.com/sonkeehoon/Docker/assets/81700507/2a025e4c-daa9-4664-ad32-2073d69db700)
- 5:05 부터

  



<hr><br>
