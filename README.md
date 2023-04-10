![image](https://user-images.githubusercontent.com/81700507/230723145-e37e88d4-8d90-4044-9e03-136df1b943b5.png)
<h1>Docker를 혼자 공부하면서 기록을 남기는 곳</h1>
<p>참고 영상 : 생활코딩 Docker 입문 수업<br>
https://www.youtube.com/playlist?list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf</p>
<h3> 이 폴더를 만들게된 계기</h3>

- 몇개월 전에 국비지원 과정에서 docker를 배우기는 했었다
- 그런데 거의 6개월 정도 사용을 안하다보니 사용법을 많이 까먹어 버렸다
- 그래서 배웠던걸 까먹지 않고 리마인드 하면서 내용도 정리하자는 취지로 이런 문서를 작성하게 됐다

<br>

- 실습 환경 : AWS EC2 인스턴스(ubuntu os)에 vscode remote ssh로 접속해서 docker를 설치했다
- 설치 방법 : https://docs.docker.com/engine/install/ubuntu/
- 1,2강은 수업소개 및 설치 강의이기 때문에 내용 정리는 생략했다
<hr>

## `3강` image pull
<p>
<center><img width = 500 src="https://user-images.githubusercontent.com/81700507/226579856-8ce93236-bf63-4c1b-8c1a-398146607d87.png"></center>
<p><이미지 출처 : 생활코딩 Docker 입문수업 - 3. 이미지 pull></p>
  
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

## `4강` container run
<p>
run : image를 실행시켜서 container를 만드는 행위 <br>
sudo docker run httpd <br>
<img width = 600 src="https://user-images.githubusercontent.com/81700507/226902543-6a84b71d-24e5-462a-8d6e-b7bc42041423.png"><br><br>
실행중인 컨테이너를 확인하려면 새로운 터미널 창에서<br>
sudo docker ps <br>
<img width = 600 src="https://user-images.githubusercontent.com/81700507/226903492-0978e9c1-3502-4d12-a2df-f63422ac64d5.png"><br><br>
컨테이너 이름을 "webserver"로 지정해서 만들려면 : sudo docker run --name webserver httpd<br>
실행중인 docker를 끄고싶을때 : sudo docker stop webserver<br>
모든 컨테이너 목록을 보고싶을때 : sudo docker ps -a<br>
# stop 했다고 container가 사라지는것이 아니다!<br>
<img width = 700 src="https://user-images.githubusercontent.com/81700507/226907492-d44acb64-2821-4d4d-a12b-e47b90507317.png"><br><br>
중지된 webserver를 다시 실행하려면 : sudo docker start webserver<br>
로그를 보려면 : sudo docker logs webserver<br>
실시간으로(docker run 했을때처럼) 로그를 보고싶으면 : sudo docker logs -f webserver<br>
컨테이너를 삭제하는 명령어 : sudo docker rm webserver<br>
그런데 컨테이너를 삭제하려는데 에러가 떴다<br>
<img width = 700 src="https://user-images.githubusercontent.com/81700507/226910668-15efa968-c5ba-43e9-bbc7-440ee3487b33.png"><br><br>
실행중인 컨테이너를 삭제하려고 했기때문에 에러가 뜬것이다<br>
실행중인 컨테이너를 먼저 stop 하고 삭제하면 된다<br>
sudo docker stop webserver<br>
sudo docker rm webserver<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/226911771-079983db-1d5c-4833-ae5e-3c8f1376d000.png"><br><br>
만약 실행중인 컨테이너를 <ins>강제로 삭제</ins>하고 싶다면 : sudo docker rm --force [container]<br>
docker <ins>image를 삭제</ins>하고 싶다면 : sudo docker rmi [image]<hr><br></p>

## `5강` network
<p>
5강은 실습이나 명령어 보다는 이론 위주의 강의였다<br><br>
<img width = 1000 height = 500 src="https://user-images.githubusercontent.com/81700507/227770613-e0286430-f638-48a4-ba0e-b42a03d8ca55.png">
<p><이미지 출처 : 생활코딩 Docker 입문수업 - 5. 네트워크 pull></p>
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

## `6강` 명령어 실행(<ins>컨테이너 내부</ins> 진입)
<p>
6강은 <ins>컨테이너 안</ins>으로 들어가서 명령을 실행하는 방법을 알아보려고 한다<br>
컨테이너 안에서 명령어 실행 : docker exec [OPTIONS] CONTAINER COMMAND [ARG...]<br>
docker exec ws3 ls<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/229092635-d50fc894-f710-47d2-b5a8-c139f8d0637a.png">
그런데 컨테이너와 <ins>지속적으로</ins> 연결을 유지하면서 계속 명령을 전달하고 싶어졌다 그럴때는<br>
docker exec -it ws3 /bin/sh <br>
-it 옵션의 i는 --interactive t는 --tty를 의미한다<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/229094552-c8e57aab-fd2b-44ad-a44f-a9e4979ff2e1.png">
컨테이너 안으로 성공적으로 들어왔다<br>
컨테이너 밖으로 나가고 싶다면 : exit<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/229095610-564295ab-22dd-40e4-8c9f-8bd668f32cef.png">
host로 돌아왔다<br>
그리고 컨테이너에서 /bin/sh로 본쉘을 실행했는데 이고잉 강사님이 본쉘을 실행시킨 이유는<br>
컨테이너 안에 bash쉘이 없는 경우가 있기 때문이라고 하셨다<br>
하지만 본쉘은 기능이 제한적이기 때문에 처음에는 /bin/bash로 실행하고<br>
실행이 안되면 /bin/sh로 실행하면 될것 같다<br>
아무튼 접속을 했고 이제 <ins>웹페이지를 수정</ins>해보자<br>
처음에는 /usr/local/apache2 경로가 설정되어있다<br>
<img width = 300 src="https://user-images.githubusercontent.com/81700507/229098516-ce81b109-ec73-49cc-9c74-2db8eb3ee912.png">
httpd 컨테이너의 index.html 파일위치는 https://hub.docker.com/_/httpd 에서 확인 가능하다 <br>
위 링크에 <ins>/usr/local/apache2/htdocs</ins>라고 설명이 나와있으니 그 경로로 이동하면 된다<br>
그리고 문서 편집을 해야 하는데 컨테이너는 기본적으로 가벼운 무게로 만들어지기 때문에<br>
vi나 nano같은 에디터가 기본설치가 안되어있어서 설치를 해야한다<br>
우선 apt update 부터<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/229100113-36909a5d-a2d7-4a1c-bcd9-11aad90e6b81.png">
그리고 설치를 진행<br>
apt install -y vim <br>
설치가 끝나면 실행하자<br>
vi index.html<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/229100666-43126051-413f-442f-92e5-2cd97da50794.png">
웹페이지 소스를 찾았다<br>
Hello, Docker!로 내용을 바꿔보자<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/229101058-273c4b3d-78e4-4964-8a43-ff817e5186c6.png">
<img width = 500 src="https://user-images.githubusercontent.com/81700507/229101500-dd8a65bb-e153-4a8b-b98c-6be5119872c5.png">
변경사항이 적용 됐다!<hr><br>
</p>

## `7강` 호스트와 컨테이너의 <ins>파일시스템</ins> 연결
<p>
지금까지는 컨테이너 안에 접속해서 직접 파일을 수정했었다.<br>
그런데 이런 방식은 불편하고 위험한 일을 초래할수도 있다<br>
예를들어 현재 상태에서 <ins>container가 삭제</ins>된다면 index.html 파일 내용까지 전부 날라가 버린다 (엄청난 재앙!)<br>
물론 컨테이너를 삭제하지 않으면 된다! 하지만..<br>
우리가 컨테이너를 쓰는 궁극적인 이유는 필요할때 <ins>언제든 생성</ins>하고 필요 없을때는 <ins>언제든 삭제</ins>할수 있기 때문이다<br>
그래서 호스트의 웹페이지(/var/www/html/index.html)의 수정사항을 <br>
컨테이너 속 웹페이지 경로(/usr/local/apache2/htdocs)에도 반영되게 하는것이 목적이다<br>
이게 된다면 컨테이너가 날라가도 우리의 소스코드는 호스트에 그대로 남아있기 때문에 보다 안전한 개발이 가능하다<br>
실행은 컨테이너에게 맡기고 개발 및 수정은 호스트에서 진행할수 있게 보자<br>
우선, 이번엔 다른포트(<ins>8888번</ins>)를 사용할것이기 때문에 AWS콘솔에 가서 포트를 열어주자<br>
<img width = 1000 src="https://user-images.githubusercontent.com/81700507/230721101-8eb70ef7-9d69-4bf1-b8f0-b74394605872.png">
ec2 인스턴스의 보안그룹을 수정해서 포트를 열어줬다<br>
이제 8888번 포트를 연결할 새 컨테이너를 만들자<br>
<ins>host의 웹페이지 위치</ins>와 <ins>컨테이너의 웹페이지 위치</ins>가 연결된 새 컨테이너를 만드는 명령어<br>
sudo docker run -p 8888:80 -v /var/www/html:/usr/local/apache2/htdocs httpd <br>
<img width = 1000 src="https://user-images.githubusercontent.com/81700507/230721473-7caec4ef-8463-4756-8e56-9dc9dd4d961f.png">
<ins>퍼블릭IP:8888</ins>로 접속해보니 <ins>퍼블릭IP:80</ins>과 동일한 웹페이지가 나왔다(성공!)<br>
이제 호스트의 index.html을 수정해서 버전관리를 할수있고<br>
vscode등의 에디터를 사용해서 호스트의 index.html을 수정해도 컨테이너 안의 내용까지 바뀐다<br>
실제 현업에서 웹페이지를 만들고 테스트 할때는 이런방식이 매우매우 유용할것 같다<br>
<hr><br>
</p>

## `2023-04-08` docker 기초내용 마무리
- 지금까지 docker의 기초적인 내용을 정리해놓았다. docker를 쓸일이 생기면 이 글을 참고할 생각이다
- 지금 다시 읽어보니 나름 정리를 잘 해놓은것 같아서 뿌듯하다
- 그리고 조금 더 고급기능을 공부하게 되면 하단에 이어서 내용을 추가할 예정이다
- <strong>docker를 더 많이 공부해보고 싶다면</strong> https://seomal.com/map/1/129

<hr><br>


















