<h2>Docker를 혼자 공부하면서 기록을 남기는 곳</h2>
<p>참고 영상 : 생활코딩 Docker 입문 수업<br>
  https://www.youtube.com/playlist?list=PLuHgQVnccGMDeMJsGq2O-55Ymtx0IdKWf</p>
<p>AWS EC2 인스턴스의 ubuntu에 vscode remote ssh로 접속해서 설치했다. <br>
설치 방법 : https://docs.docker.com/engine/install/ubuntu/<br>
1,2강은 수업소개 및 설치 강의이기 때문에 내용 정리는 생략했다</p>

<p><h3>3강 image pull</h3>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/226579856-8ce93236-bf63-4c1b-8c1a-398146607d87.png">
<p"><이미지 출처 : 생활코딩 Docker 입문수업 - 3. 이미지 pull></p><br>
  
app store에서 program을 다운 받는다 => <strong>docker hub</strong>에서 <strong>image</strong>를 다운받는다<br>
program을 실행하면 process가 동작 => <strong>image</strong>를 실행하면 <strong>container</strong>가 동작<br>
program이 여러개의 프로세스를 가질수 있듯, image도 여러개의 컨테이너를 가질수 있음<br>
docker hub에서 image를 다운받는 행위를 <strong>pull</strong>, image를 실행시키는 행위를 <strong>run</strong> 이라고 한다<br>
필요한 image를 다운받기 위해 hub.docker.com으로 접속 후 Explore 버튼 클릭<br>
apache웹서버를 컨테이너 위에서 실행시키기 위해 httpd 검색(docker hub상에서 apache웹서버는 httpd라는 이름을 갖는다)<br>
docker pull 명령어 설명 : https://docs.docker.com/engine/reference/commandline/pull/ <br><br>
sudo docker pull httpd (apache 웹서버의 docker image 다운로드)<br>
나는 리눅스 os에서 실행하니까 sudo를 붙였다<br>
mac os의 경우 모든 명령어에서 sudo를 빼면 된다고 한다 <br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/226581437-458aaf00-73ff-4255-9a38-4c89ecf9fecb.png"><br>
sudo docker images (성공적으로 만들었는지 확인)<br>
<img width = 500 src="https://user-images.githubusercontent.com/81700507/226582323-c083a92d-8591-4cbd-bbfa-00bf095f3c82.png">

</p>



