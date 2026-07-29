# codessey
코디세이 과제를 위한 레포입니다.
## 1.프로젝트 개요(미션 목표 요약)

내 컴퓨터에 개발자용 '작업실' 꾸미기.

## 2. 실행환경(OS/쉘/터미널, Docker 버전, Git버전)
OS: ubuntu 24.04

Shell : bash

Docker :  29.6.2

Git : 2.45.2

---

## 3.수행 항목 체크리스트(터미널/권한/Docker/Dockerfile/포트/마운트/볼륨/Git/Github)

- [O] 터미널 기본 조작 및 폴더 구성
- [O] 권한 변경 실습
- [O] Docker 설치/점검
- [O] hello-world 실행
- [x] Dockerfile 빌드/실행
- [x] 포트 매핑 접속(2회)
- [x] 바인드 마운트 반영
- [x] 볼륨 영속성
- [O] Git 설정 + VSCode GitHub 연동

---

## 4. 터미널 조작 로그 기록

#### 현재 위치 확인

```bash
$ pwd
/home/code
```

#### 목록 확인(숨김 파일 포함)
```bash
$ ls -al
snap 공개 다운로드 문서 바탕화면 비디오 사진 서식 음악
```

#### 생성, 파일 내용 확인, 이동, 복사
```bash
#test1 디렉터리 생성
$ mkdir test1
$ ls -al
test1 snap 공개 다운로드 문서 바탕화면 비디오 사진 서식 음악

#test_file 빈파일 생성
$ cd test1
$ touch test_file.txt
$ ls
test_file

#파일 내용 추가 후 확인
$ vi test_file.txt
#vi 에디터 활용
#i 누르고 hello 입력 후 :wq 입력
$ cat test_file.txt
hello

# 복사
$ cp test_file.txt /home/copy_test_file.txt
$ cd ../
# 최상위 디렉터리의 home 이기 때문에 절대경로 사용
$ cd /home
$ ls
code copy_test_file.txt


```

#### 이동/이름 변경
```bash
# 파일 이름 변경
$ mv test_file.txt mv_test_file.txt
$ ls
mv_test_file.txt

# 파일 이동
$ mv test_file.txt /home/code/mv_test_file.txt
$ cd ../
$ cd /test2 ; ls
$ mv_test_file.txt

```

#### 삭제
```bash
# 파일 삭제
$ rm  mv_test_file.txt ; ls

# test2 디렉터리 삭제 -d옵션 활용
$ rm -d test2 ; ls
snap test1 공개 다운로드 문서 바탕화면 비디오 사진 서식 음악

# test1 디렉터리가 비어 있지 않아 삭제가 안됩니다.
$ rmdir test1 ; ls
rmdir: 'test1' 제거 실패: 디렉터리가 비어있지 않음

# 숨김 파일 존재 확인
$ cd test1 ; ls -al
-bash: cd: test1: 그런 파일이나 디렉터리가 없습니다
합계 20
drwxrwxr-x  2 code code  4096  7월 28 00:25 .
drwxr-x--- 17 code code  4096  7월 28 08:40 ..
-rw-r--r--  1 code code 12288  7월 27 23:37 .test_file.swp

# rm -rf 옵션으로 내부 내용까지 한 번에 삭제
$ rm -rf test1 ; ls
snap  공개  다운로드  문서  바탕화면  비디오  사진  서식  음악

```

## 5. 권한 실습 및 증거 기록

권한 실습 및 증거 기록에 앞서 이 내용을 수행하는데 필요한 기본 지식부터 소개하겠습니다.<br>

리눅스 시스템에 있는 모든 파일과 디렉터리에서는 그것을 엑세스 할 수 있는 소유자와 그룹에 대한 소유권을 가집니다.<br>
이런 파일과 디렉터리에 엑세스 할 수 있도록 퍼미션(권한)으로 접근을 제어할 수 있으며 보통 계정 이름으로 표기되거나 어떤 경우에는 UID로 표기되기도 합니다.<br>

### 퍼미션 형식 구조

  -8진수 (r:4 ,w:2 ,x:1의 값을 가진다)
  -r:읽기 / w:쓰기 / x: 실행 허용

| 파일유형 | 사용자(user) | 그룹 | 기타 |
| :--- | :--- | :--- | :--- |
| **-** | r  w  x | r  w  x | r  w  x |

ex)777권한을 가진다.(rwxrwxrwx)==> 사용자ㆍ그룹ㆍ기타 모두 읽기와 쓰기 실행 허용 권한을 가진다.<br>
ex)644권한을 가진다.(rw-r--r--) ==> 사용자는 읽기와 쓰기 / 그룹과 기타는 읽기 권한만 가진다!<br>


#### 파일 권한 변경 실험

```bash
$ touch file.txt ; ls -l

-rw-r--r-- 1 root root    0  7월 28 09:54 file.txt
# 맨앞 - : 파일 종류 중 일반 정규 파일을 의미
# 허가권한: rw-r--r-- : 644 퍼미션 형식 구조로 8진수(r(읽기):4, w(쓰기):2, x(실행허용):1)로 표시됩니다.
# 앞에서부터 3개는 사용자, 그룹, 기타 건한을 의미하며 644는 즉, 사용자는 읽기 쓰기, 그룹과 기타는 읽기 권한만 가지는 것 입니다.
# 1: 링크 수
# root: 그룹명
# root: 그룹명
# 0 : 파일크기(아무것도 작성하지 않았습니다.)
# 7월 28 09:54 : 마지막 변경된 시간과 날짜 
# file.txt : 파일 이름

# 파일의 사용자(소유자) 변경
$ sudo chown code file.txt ; ls -l

-rw-r--r-- 1 code root    0  7월 28 09:54 file.txt

#파일의 그룹 변경
$ sudo chgrp code file.txt ; ls -l

-rw-r--r-- 1 code code    0  7월 28 09:54 file.txt

# 소유자, 그룹, 기타 권한 변경
$ chmod 777 file.txt ; ls -l
-rwxrwxrwx 1 code root    0  7월 28 09:54 file.txt

```
소유자, 그룹, 기타 사용자 모두의 권한이 777 즉 읽기, 쓰기, 실행파일 허용의 권한이 모두 주어졌습니다.

#### 디렉터리 권한 확인
```bash
$ mkdir test1 ; ls -l

drwxr-xr-x 2 root root 4096  7월 28 09:53 test1

```

## 6. 도커 설치 및 기본 점검

```bash
docker version

Client:
 Version:           29.6.2
 API version:       1.55
 Go version:        go1.26.5
 Git commit:        
 Built:             Thu Jul 16 16:14:59 2026
 OS/Arch:           windows/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.83.0 ()
 Engine:
  Version:          29.6.2


docker info

Client:
 Version:    29.6.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)

```

## 7. 도커 기본 운영 명령 수행

```shell
# nginx라는 웹 서버 이미지를 내 컴퓨터로 다운로드
docker pull nginx

Status: Downloaded newer image for nginx:latest

# my-web이라는 이름의 컨테이너를 생성 
docker run -d -p 80:80 --name my-web nginx 

# -d : 데몬 모드(백그라운드에서 실행)
# -p : 80:80 내 컴퓨터의 80 번 포트와 컨테이너의 80 번 포트를 연결.

# 현재 실행중인 컨테이너 확인
docker ps 

CONTAINER ID   IMAGE     COMMAND    CREATED    STATUS     PORTS     NAMES
nginx     "/docker-entrypoint.…"   35 seconds ago   Up 33 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   my-web

# 중지된 컨테이너까지 확인
docker ps -a

CONTAINER ID   IMAGE          COMMAND                   CREATED             STATUS                         PORTS                                 NAMES
   nginx          "/docker-entrypoint.…"   50 seconds ago      Up 48 seconds                  0.0.0.0:80->80/tcp, [::]:80->80/tcp   my-web
   ubuntu:24.04   "/bin/bash"               About an hour ago   Exited (0) About an hour ago                                         cool_bhaskara


# 실행중인 컨테이너 중지
docker stop my-web

# 중지된 컨테이너 실행
docker start my-web

#컨테이너 삭제
docker rm -f my-web
#다운로드 했던 이미지 삭제
docker rmi nginx

#모든 컨테이너 제거
docker rm $(docker ps -aq)

# 317cfb683799 컨테이너의 로그를 최근 10줄만 확인

PS C:\WINDOWS\system32> docker logs --tail 10 317cfb683799
drwxr-xr-x  12 root root 4096 Jun 10 02:05 usr/
drwxr-xr-x  11 root root 4096 Jun 10 02:12 var/
root@317cfb683799:/# echo test1
test1
root@317cfb683799:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@317cfb683799:/# exit
exit
root@317cfb683799:/# exit
exit

#stats 명령어를 활용해 nginx 컨테이너 리소스 확인
docker stats -a 0463441f4828

CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O     PIDS
0463441f4828   my-web    0.00%     16.96MiB / 7.517GiB   0.22%     1.17kB / 126B   0B / 12.3kB   19

#states -a 옵션을 활용해서 중지된 컨테이너 확인
docker stats -a 317cfb683799

CONTAINER ID   NAME                 CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
317cfb683799   friendly_heyrovsky   0.00%     0B / 0B             0.00%     0B / 0B   0B / 0B     0

```
## 8. 컨테이너 실행 실습

```shell
# hello-world 실행 성공을 기록
docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

# 우분투 이미지 받고 /bin/bash 쉘로 실행
# -it : 컨테이너 안의 터미널과 내 키보드/화면을 연결해서 상호작용하기 위한 옵션.
docker run -it ubuntu:24.04 /bin/bash
root@xxxx:/#

#ls 명령어 실행 결과
root@xxxx:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

#echo 명령어 실행 결과
root@xxxx:/# echo test1
test1

# 컨테이너 관찰
# exit(컨테이너 정지)
exit
docker ps
root@xxxx:/# exit

exit

PS C:\WINDOWS\system32> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

#컨테이너의 main 프로세스(bash)를 종료하니 컨에너도 함께 정지.
PS C:\WINDOWS\system32> docker ps -a
CONTAINER ID   IMAGE          COMMAND         CREATED          STATUS                      PORTS     NAMES
317cfb683799   ubuntu:24.04   "/bin/bash"     5 minutes ago    Exited (0) 26 seconds ago             friendly_heyrovsky

# attach명령어로 실행중인 컨테이너의 메인 화면으로 연결
PS C:\WINDOWS\system32> docker start 317cfb683799
317cfb683799

PS C:\WINDOWS\system32> docker ps

CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS          PORTS     NAMES
317cfb683799   ubuntu:24.04   "/bin/bash"   9 minutes ago   Up 10 seconds             friendly_heyrovsky

PS C:\WINDOWS\system32> docker attach 317cfb683799
root@317cfb683799:/#

# Ctrl + P, Q(컨테이너 유지-Detach)
# 컨테이너는 실행 되는 상태로 내 터미널만 나오기.

#Ctrl + P ,Q
root@317cfb683799:/# read escape sequence

docker ps
# 컨테이너가 실행 중임을 알 수 있습니다.
PS C:\WINDOWS\system32> docker ps

CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS         PORTS     NAMES
317cfb683799   ubuntu:24.04   "/bin/bash"   12 minutes ago   Up 3 minutes             friendly_heyrovsky

# exec로 실행 중인 컨테이너에 들어가기
# 즉, 샐행 중인 컨테이너에 새로운 문을 하나 더 열고 들어가기
# it를 꼭 사용해야 된다.

PS C:\WINDOWS\system32> docker exec -it 317cfb683799 /bin/bash
root@317cfb683799:/# exit
exit

# exit로 나와도 컨테이너가 실행 중인 것을 알 수 있다.
# 문이 2개인데 1개만 닫았기 때문입니다.
PS C:\WINDOWS\system32> docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS         PORTS     NAMES
317cfb683799   ubuntu:24.04   "/bin/bash"   14 minutes ago   Up 5 minutes             friendly_heyrovsky

```

| 구분 | 명령어 | 컨테이너 상태 변화 | 비유 |
| :--- | :--- | :--- | :--- |
| 종료 | exit | 종료(Exited) | 방의 불을 끄고 나감 |
| 유지(탈출) | Ctrl+p,q | 실행 중(up) | 방의 불을 켜둔 채 몸만 나옴 |
| 재진입(attach) | docker attach | 연결됨 | 이미 켜진 TV 앞에 다시 앉음 |
| 추가실행(exec) | docker exec | 연결됨(새 프로세스) | 방에 다른 문을 열고 들어감 |

## 9. 기존 도커파일 기반 커스텀 이미지 제작

### (A) 웹 서버 베이스 이미지 활용

#### 선택한 베이스 이미지
선택한 베이스 이미지는 **nginx:latest** 입니다.
**선택 이유**: 가장 가볍고 널리 쓰이는 웹 서버 이미지이며, 정적 파일(HTML)만을 교체할 때 커스텀 **결과를 즉각 확인**하기 좋아서 선택했습니다.

#### 커스텀 포인트 및 목적
파일 교체 (index.html): NGINX의 기본 시작 페이지 대신, 무엇을 위한 페이지인지 알려주기 위함입니다.
포트 포워딩 설정: 호스트(내 컴퓨터)의 8080 포트와 컨테이너의 80 포트를 연결하여 웹 브라우저에서 접속 가능하게 했습니다.

#### 빌드/실행 명령 + 핵심결과

1. web_base라는 폴더를 만들고 폴더 안에 간단한 정적 index.html 파일을 만듭니다.
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>My Custom Docker Image</title>
</head>
<body>
    <h1>안녕하세요! 도커 커스텀 이미지 실습 중입니다.</h1>
    <p>이 페이지는 NGINX 베이스 이미지에 제가 만든 정적 파일이 포함된 결과물입니다.</p>
</body>
</html>
```
2. 같은 폴더 안에 Dockerfile 을 만들고 다음과 같이 작성해줍니다.

```Dockerfile
# 1. 베이스 이미지 선택
FROM nginx:latest

# 2. 커스텀 포인트: 내가 만든 index.html을 컨테이너 안의 특정 경로로 복사
# NGINX는 기본적으로 /usr/share/nginx/html 경로의 파일을 웹에 띄워주기에 아래와 같이 작성해줬습니다.
COPY index.html /usr/share/nginx/html/index.html

# 3. (선택) 컨테이너가 80번 포트를 사용함을 명시
EXPOSE 80
```

3. webshell

```shell

# 도커 이미지 빌드
PS C:\web_base> docker build -t web_base:v1 .

[+] Building 1.0s (7/7) FINISHED                                                                                                                                                              docker:desktop-linux


 => => unpacking to docker.io/library/web_base:latest                                                                                                                                                         0.1s
# web_base:v1 . 에서 v1은 기존 이미지인 web_base에 대한 태그 참조를 새로운 태그와 함께 저장.<br>
# .은 현재 경로를 의미 / 즉, 현재 경로에 있어도 . 을 사용하지 않으면 이미지 업로드가 실패합니다!<br>
# 반대로 web_base 경로가 아닌 다른 경로에 있는데 . 을 사용해도 이미지 업로드가 실패 합니다!<br>

# 실패 코드와 오류 내용
PS C:\code> docker build -t web_base:v1 .
[+] Building 0.2s (1/1) FINISHED                                                                                                                                                              docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                          0.1s
 => => transferring dockerfile: 2B                                                                                                                                                                            0.0s
ERROR: failed to build: failed to solve: failed to read dockerfile: open Dockerfile: no such file or directory

#dockerfile이 없어 찾지 못하니 당연히 실패했던 것 같습니다.



# 컨테이너 실행

PS C:\web_base> docker run -d -p 8080:80 --name my-web-container web_base:v1
df7e210e37da9352f074de279ff0324c18ff3356c425bf242b1e600a05e5a862

#컨테이너 상태가 up 임을 확인했습니다.<br>
# localhost로 들어가서 확인해보니 정상적으로 사이트가 로드되었습니다.<br>
docker ps
CONTAINER ID   IMAGE      COMMAND                   CREATED          STATUS          PORTS                                     NAMES
df7e210e37da   web_base   "/docker-entrypoint.…"   8 seconds ago    Up 7 seconds    0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web-container
```

### (B) Linux 베이스 이미지

#### 선택한 베이스 이미지
ubuntu:22.04 (익숙한 리눅스 환경을 구축하기 위해 선택)

#### 커스텀 포인트 및 목적:

패키지(curl, vim) 설치: 컨테이너 내부에서 네트워크 테스트 및 파일 편집과 같은 기본 기능을 가능하게 했습니다.<br>
사용자(student) 추가: root 권한이 아닌 일반 사용자 계정을 사용하여 보안성을 향상시켰습니다.<br>
환경 변수(ENV): 애플리케이션의 이름과 버전을 관리하기 쉽게 설정하였습니다.<br>
헬스체크(HEALTHCHECK): 컨테이너의 네트워크 연결 상태를 주기적으로 감시하기 사용했습니다.<br>

#### 빌드/실행 결과:

docker build 과정에서 패키지 설치 로그 확인.<br>
docker ps를 통해 healthy 상태 및 student 계정 접속 확인.<br>

1. linux_base 폴더를 만들고 Dockerfile을 작성해주었습니다.<br>

```Dockerfile
#베이스 이미지지정 ubuntu:22.04로 지정
FROM ubuntu:22.04

# 환경변수 지정(이미지 안에서 사용할 변수)
#MystudyApp 이름으로 버전은 1.0.0으로 지정
ENV APP_NAME="MystudyApp"
ENV APP_VERSION="1.0.0"

# RUN: 명령을 실행하여 새 이미지에 포함
# 패키지 설치 (중간에 Y/n묻지 않게 -y 지정)
RUN apt-get update && apt-get install -y \
    curl \
    vim \
    && rm -rf /var/lib/apt/lists/*


#root 계정으로 로그인시 보안 상 위험 존재(path traversal)
# student라는 사용자명을 만들고 그 명으로 로그인하도록 함.
RUN useradd -m student
USER student

# 작업 디렉터리 설정(로그인 시 바로 이동하 위치)
WORKDIR /home/student

# 헬스체크 (컨테이너가 주기적으로 잘 작동하는지 확인)
# 30초마다 curl명령어로 구글에 잘 접속되는지 확인
# 구글 주소 이유: 상시 가용성, 인터넷 연결 검증(사내망 넘어 실제 WAN), DNS 정상 작동 확인
HEALTHCHECK --interval=30s --timeout=3s \
 CMD curl -f https://www.google.com || exit 1

# CMD : 컨테이너가 시작될 때 실행할 커맨드를 지정
CMD ["sleep", "3600"]
```


2. shell에서 리눅스 기반 도커 이미지를 빌드하고 컨테이너를 실행하였습니다.

```shell

#linux_base 도커 이미지 빌드

PS C:\code> cd linux_base
PS C:\linux_base> docker build -t linux_base:v1 .
[+] Building 28.6s (9/9) FINISHED                                                                                                                                                             docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                          0.1s
 => => transferring dockerfile: 1.12kB

 # 컨테이너 실행 및 상태 확인
 PS C:\linux_base> docker run -d --name my-linux-container linux_base:v1
f1a4a9f68bae0c6761b317d809a513c6fdc65607491748ca3dd7fbc76e732c4e

PS C:\linux_base> docker ps
CONTAINER ID   IMAGE           COMMAND                   CREATED          STATUS                    PORTS                                     NAMES
f1a4a9f68bae   linux_base:v1   "sleep 3600"              15 minutes ago   Up 15 minutes (healthy)                                             my-linux-container

# 컨테이너 실행 후 설정 확인
PS C:\Users\yangh\code\linux_base> docker exec -it f1a4a9f68bae /bin/bash
student@f1a4a9f68bae:~$
```

#### curl 명령어 실행을 위한 한경 세팅 변경

```dockerfile
CMD ["python3", "-m", "http.server", "8080"]
```

이후 이전 컨테이너 삭제 후 이미지를 다시 build 해야 합니다.<br>
저는 이미지를 다시 빌드 하지 않아서 많은 길을 돌아갔습니다.<br>

#### 이전 컨테이너 삭제 후 다시 빌드 후 실행

```dockerfile
5dee01bfcfaf   cafda12624dd    "sleep 3600"              2 hours ago         Exited (255) 33 seconds ago                            my-linux-container2
787a5d37c424   cafda12624dd    "sleep 3600"              2 hours ago         Exited (137) 2 hours ago                               my-linux-container

# -t 옵션으로 이름을 linux_base:v1으로 지정하여 다시 빌드
docker build -t linux_base:v1 .

# 새로 빌드된 이미지로 실행. 이때 -p 옵션으로 포트 설정 추가!
docker run -d -p 8081:8080 --name my_linux_container linux_base:v1

docker ps
CONTAINER ID   IMAGE           COMMAND                   CREATED       STATUS                        PORTS                                         NAMES
93ab17eef57f   linux_base:v1   "python3 -m http.ser…"   2 hours ago   Up 8 minutes (healthy)        0.0.0.0:8081->8080/tcp, [::]:8081->8080/tcp   my_linux_container

```
#### curl 명령어 실행 확인

```shell
student@93ab17eef57f:~$ curl http://localhost:8080

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>Directory listing for /</title>
</head>
<body>
<h1>Directory listing for /</h1>
<hr>
<ul>
<li><a href=".bash_logout">.bash_logout</a></li>
<li><a href=".bashrc">.bashrc</a></li>
<li><a href=".profile">.profile</a></li>
</ul>
<hr>
</body>
</html>
```

## 10. 포트 매핑 및 접속 증거

| 항목 | 내용 |
| :--- | :--- |
| **베이스 이미지** | `nginx:latest` |
| **커스텀 목적** | 기본 NGINX 페이지를 사용자 정의 HTML 파일(`src/index.html`)로 교체하여 웹 서비스 배포 |
| **핵심 명령어** | `docker build -t my-web-app:v1 .`<br>`docker run -d -p 8080:80 my-web-app:v1` |
| **포트 매핑** | 호스트 8080번 포트와 컨테이너 80번 포트를 연결하여 외부 접속 허용 |
| **결과** | 브라우저에서 `localhost:8080` 접속 시 커스텀 페이지 정상 출력 확인 |

### Dockerfile 이미지 생성 및 기본 세팅 과정

#### 1. 프로젝트 구조 
my_web_server/
├── src/
│   └── index.html
└── Dockerfile

#### 2.소스코드 및 Dockerfile 내용

**src/index.html**
웹 서버에 띄울 실제 콘텐츠입니다.
웹 서버에 띄울 때 `<meta charset="UTF-8">` 을 입력해줘야 브라우저가 착각하지 않고 인코딩하여 한글이 깨지지 않고 나옵니다!
```html
<<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8"
    <title>도커 실습 페이지</title>
    <style>
        body { background-color: #f0f8ff; text-align: center; padding-top: 50px; font-family: sans-serif; }
        h1 { color: #2c3e50; }
    </style>
</head>
<body>
    <h1> !!Docker 커스텀 이미지 빌드 성공!</h1>
    <p>이 페이지는 NGINX 컨테이너 내부에서 실행 중입니다.</p>
    <p>포트 매핑: <strong>8080(Host) -> 80(Container)</strong></p>
</body>
</html>
```

**Dockerfile**
이미지를 만드는 설정입니다.

```dockerfile
FROM nginx:latest

커스텀 포인트: 로컬의 src 폴더 내용을 컨테이너의 웹 루트 경로(가장 많이 쓰는 경로)로 복사
COPY src/ /usr/share/nginx/html/

#80번 포트 개방 명시
EXPOSE 80
```

#### 3. 접속화면
```shell
docker build -t my_web_server:v1 .
[+] Building 2.2s (8/8) FINISHED 

docker run -d -p 8080:80 --name my-web-server-container my_web_server:v1
9dee0a3233509053fe13a7564644ecd4aa190d9e3f16359bda68f49a075f6bf3

PS C:\my_web_server> docker ps
CONTAINER ID   IMAGE              COMMAND                   CREATED         STATUS         PORTS                                     NAMES
9dee0a323350   my_web_server:v1   "/docker-entrypoint.…"   2 seconds ago   Up 2 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web-server-container
```

<img width="855" height="302" alt="Image" src="https://github.com/user-attachments/assets/c7b0d8eb-2bfa-4bb4-a600-df96aa6196c6" />

#### 4. 추가 ubuntu 베이스로 포트 매핑

| 항목 | 내용 |
| :--- | :--- |
| **베이스 이미지** | `ubuntu:22.04` |
| **커스텀 목적** | Ubuntu에 NGINX를 직접 설치하고 커스텀 HTML(`src/index.html`)을 배치하여 웹 서비스 배포 |
| **핵심 명령어** | `docker build --no-cache -t ubuntu_nginx_web:v3 -f Dockerfile.ubuntu .`<br>`docker run -d -p 8081:80 --name ubuntu_nginx_container ubuntu_nginx_web:v3` |
| **포트 매핑** | 호스트 8081번 포트와 컨테이너 80번 포트를 연결하여 외부 접속 허용 (`8081:80`) |
| **결과** | 브라우저에서 `localhost:8081` 접속 시 "Docker B 형식 성공!" 커스텀 페이지 출력 확인 |

**index.html**
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Docker B 형식 테스트</title>
</head>
<body>
    <h1>Docker B 형식 성공!</h1>
    <p>이 페이지는 Ubuntu 베이스 이미지에 NGINX를 직접 설치해서 실행 중입니다.</p>
    <p>포트 매핑: 8081(Host) → 80(Container)</p>
</body>
</html>
```

**Dockerfile.ubuntu**
```Dockerfile
FROM ubuntu:22.04

#ubuntu안에 nginx 직접 설치
RUN apt-get update && \
    apt-get install -y nginx && \
    rm -rf /var/lib/apt/lists/*

#내 컴퓨터의 HTML파일을 컨테이너 안의 NGINX 기본 웹 루트로 복사
COPY src/index.html /var/www/html/index.html

EXPOSE 80
#NGINX가 백그라운드로 빠지지 않고 컨테이너의 메인 프로세스로
#계속 실행
#deamon off: 메인프로세스인 NGINX가 백이 아닌 포그라운드에서 실행유지
#컨테이너 유지를 위해서
CMD ["nginx", "-g", "daemon off;"]

```

```shell
docker build --no-cache -t ubuntu_nginx_web:v3 -f Dockerfile.ubuntu .

docker run -d -p 8081:80 --name ubuntu_nginx_container ubuntu_nginx_web:v3

docker ps
CONTAINER ID   IMAGE                 COMMAND                   CREATED         STATUS         PORTS                                     NAMES
016057182e6e   ubuntu_nginx_web:v3   "nginx -g 'daemon of…"   4 seconds ago   Up 3 seconds   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   ubuntu_nginx_container

```
<img width="681" height="335" alt="Image" src="https://github.com/user-attachments/assets/2c4ce208-8d15-48a8-b5b5-612879a88a97" />




