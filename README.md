<h1>Docker를 혼자 공부하면서 기록을 남기는 곳</h1>
<p>참고 영상 : 생활코딩 Docker 입문 수업<br>
https://www.youtube.com/playlist?list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf</p>
<p>AWS EC2 인스턴스의 ubuntu에 vscode remote ssh로 접속해서 설치했다<br>
설치 방법 : https://docs.docker.com/engine/install/ubuntu/<br>
1,2강은 수업소개 및 설치 강의이기 때문에 내용 정리는 생략했다</p>
<hr><p><h2>3강 image pull</h2>
<center><img width = 500 src="https://user-images.githubusercontent.com/81700507/226579856-8ce93236-bf63-4c1b-8c1a-398146607d87.png"></center>
<p><이미지 출처 : 생활코딩 Docker 입문수업 - 3. 이미지 pull></p><br>
  
app store에서 program을 다운 받는다 => <strong>docker hub</strong>에서 <strong>image</strong>를 다운받는다<br>
program을 실행하면 process가 동작 => <strong>image</strong>를 실행하면 <strong>container</strong>가 동작<br>
program이 여러개의 프로세스를 가질수 있듯, image도 여러개의 컨테이너를 가질수 있음<br>
docker hub에서 image를 다운받는 행위를 <strong>pull</strong>, image를 실행시키는 행위를 <strong>run</strong> 이라고 한다<br>
필요한 image를 다운받기 위해 hub.docker.com으로 접속 후 Explore 버튼 클릭<br>
apache웹서버를 컨테이너 위에서 실행시키기 위해 httpd 검색(docker hub상에서 apache웹서버는 httpd라는 이름을 갖는다)<br>
docker pull 명령어 설명 : https://docs.docker.com/engine/reference/commandline/pull/ <br><br>
sudo docker pull httpd (apache 웹서버의 docker image 다운로드)<br>
리눅스 환경이기 때문에 sudo를 붙였다(mac os의 경우 모든 명령어에서 sudo를 빼면 된다)<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/226581437-458aaf00-73ff-4255-9a38-4c89ecf9fecb.png"><br><br>
sudo docker images (성공적으로 만들었는지 확인)<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/226582323-c083a92d-8591-4cbd-bbfa-00bf095f3c82.png"><br>
- image 삭제 명령어 : sudo docker rmi [image ID]<hr><br>
</p>

<p><h2>4강 container run</h2>
run : image를 실행시켜서 container를 만드는 행위 <br>
sudo docker run httpd <br>
<img width = 600 src="https://user-images.githubusercontent.com/81700507/226902543-6a84b71d-24e5-462a-8d6e-b7bc42041423.png"><br>
실행중인 컨테이너를 확인하려면 새로운 터미널 창에서<br>
sudo docker ps <br>
<img width = 600 src="https://user-images.githubusercontent.com/81700507/226903492-0978e9c1-3502-4d12-a2df-f63422ac64d5.png"><br>
컨테이너 이름을 "webserver"로 지정해서 만들려면 : sudo docker run --name webserver httpd<br>
실행중인 docker를 끄고싶을때 : sudo docker stop webserver<br>
모든 컨테이너 목록을 보고싶을때 : sudo docker ps -a<br>
# stop 했다고 container가 사라지는것이 아니다!<br>
<img width = 700 src="https://user-images.githubusercontent.com/81700507/226907492-d44acb64-2821-4d4d-a12b-e47b90507317.png"><br>
중지된 webserver를 다시 실행하려면 : sudo docker start webserver<br>
로그를 보려면 : sudo docker logs webserver<br>
실시간으로(docker run 했을때처럼) 로그를 보고싶으면 : sudo docker logs -f webserver<br>
컨테이너를 삭제하는 명령어 : sudo docker rm webserver<br>
그런데 컨테이너를 삭제하려는데 에러가 떴다<br>
<img width = 700 src="https://user-images.githubusercontent.com/81700507/226910668-15efa968-c5ba-43e9-bbc7-440ee3487b33.png"><br>
실행중인 컨테이너를 삭제하려고 했기때문에 에러가 뜬것이다<br>
실행중인 컨테이너를 먼저 stop 하고 삭제하면 된다<br>
sudo docker stop webserver<br>
sudo docker rm webserver<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/226911771-079983db-1d5c-4833-ae5e-3c8f1376d000.png"><br>
만약 실행중인 컨테이너를 강제로 삭제하고 싶다면 : sudo docker rm --force [container]<br>
docker image를 삭제하고 싶다면 : sudo docker rmi [image]<hr><br></p>

<p><h2>5강 network</h2>
5강은 실습이나 명령어 보다는 이론 위주의 강의였다<br><br>
<img width = 1000 height = 500 src="https://user-images.githubusercontent.com/81700507/227770613-e0286430-f638-48a4-ba0e-b42a03d8ca55.png">
<p><이미지 출처 : 생활코딩 Docker 입문수업 - 5. 네트워크 pull></p><br>
도커를 이용하면 웹서버가 컨테이너 안에 설치된다<br>
이 컨테이너가 설치된 운영체제를 docker host 라고 부른다<br>
하나의 호스트에는 여러개의 컨테이너가 만들어질 수 있다<br>
컨테이너와 호스트는 모두 독립적인 실행환경이기 때문에 각자 독립적인 port와 file system을 갖는다<br>
이 상태로 웹브라우저로 웹서버에 접속하려고 하면 연결이 안된다<br>
왜냐? 호스트와 컨테이너는 연결이 끊겨있기 때문<br>
그럼 어떻게할까? 호스트의 80번 포트와 컨테이너의 80번 포트를 연결해주면 된다<br>
그걸 위한 명령어 : sudo docker run -p 80:80 httpd<br>
80:80 에서 앞의 80은 host의 포트, 뒤의 80은 컨테이너의 포트<br>
이러면 호스트와 컨테이너의 포트가 서로 연결된다<br>
이렇게 연결된 포트로 신호를 전달하는것을 '포트포워딩' 이라고 한다<br>
sudo docker run [OPTIONS] IMAGE [COMMAND] [ARG..]<br>
호스트의 8080포트와 컨테이너의 80번 포트를 연결하기 : sudo docker run --name ws3 -p 8080:80 httpd
<img width = 1000 src="https://user-images.githubusercontent.com/81700507/227772234-eddfc619-4d47-45ca-993f-6644fb42b3f5.png">
컨테이너는 성공적으로 만들어졌다 그러나!<br>
퍼블릭IP:8080 로 접속하면 실행이 되야 하는데 안된다<br>
이유는 호스트(ec2 instance)의 8080포트가 열려있지 않기때문!<br>
aws ec2의 네트워크 및 보안 - 보안그룹에서 호스트pc(ec2)에 적용된 보안그룹을 선택하고 새로운 보안그룹을 추가해서 8080포트를 열어준다<br>
<img width = 1000 src="https://user-images.githubusercontent.com/81700507/227772518-95d6a2b3-a90b-4ca2-86d5-b18e295f3a02.png">
그리고 다시 http://3.39.137.68:8080/ 에 접속하면<br>
<img width = 300 src="https://user-images.githubusercontent.com/81700507/227772875-c2b9203f-db84-4743-bb97-cde71c4fa605.png">
접속에 성공했다<hr><br>
</p>



