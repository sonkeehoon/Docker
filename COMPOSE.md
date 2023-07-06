![image](https://github.com/sonkeehoon/Docker/assets/81700507/794fec8f-f6f0-46a8-8b25-3afc1ecfa866)

# Docker Compose 
Docker의 기초를 공부한 뒤 추가적인 공부를 하는공간
- 강의 자료 : https://www.youtube.com/watch?v=EK6iYRCIjYs
- 선수 과목 : Docker 기초
  - https://www.youtube.com/playlist?list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf
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

- 우선 이 링크로 접속하자
  - https://bit.ly/docker-compose-sample
  - command부분과 아래 docker-compose.yml은 같은 결과를 보여준다
- command부분을 일단 차례대로 실행해보자
  - 콘솔에서 docker network create wordpress_net
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
  -
  -
  



<hr><br>
