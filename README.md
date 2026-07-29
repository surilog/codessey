## 1.프로젝트 개요(미션 목표 요약)

내 컴퓨터에 개발자용 '작업실' 환경을 구축하고 기본 리눅스 CLI 조작 및 Docker, Git/GitHub 환경을 실습합니다.

## 2. 실행환경(OS/쉘/터미널, Docker 버전, Git버전)
OS: ubuntu 24.04 LTS / Windows 11 (Docker Desktop) / macOS (Apple Silicon / Intel)

Shell : Zsh / Bash / PowerShell

Docker :  OrbStack (Docker Engine v29.6.2 호환), 29.6.2

Git : 2.45.2

---

## 3.수행 항목 체크리스트(터미널/권한/Docker/Dockerfile/포트/마운트/볼륨/Git/Github)

- [O] 터미널 기본 조작 및 폴더 구성
- [O] 권한 변경 실습
- [O] Docker 설치/점검
- [O] hello-world 실행
- [O] Dockerfile 빌드/실행
- [O] 포트 매핑 접속(2회)
- [O] 바인드 마운트 반영
- [O] 볼륨 영속성
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

**[+]절대경로와 상대경로 알고가기**

**절대경로**: 어디서 실행하든 항상 고정된 하나의 정확한 위치를 가리킵니다.
**상대경로**: 현재 위치를 기준으로 합니다!

절대경로가 더 정확히 경로를 지정할 수 있지만 저는 사용하면서 간결하고 유연하게 사용했던 **상대경로**를 많이 사용했었던 것 같습니다.


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
# root: 사용자
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

```Powershell
# nginx라는 웹 서버 이미지를 내 컴퓨터로 다운로드
docker pull nginx

Status: Downloaded newer image for nginx:latest

# my-web이라는 이름의 컨테이너를 생성 
docker run -d -p 80:80 --name my-web nginx 

# -d : 데몬 모드(백그라운드에서 실행) , 터미널 창을 차지하지 않지만 서비스가 계속 켜져있습니다.
# -p : 80:80 내 컴퓨터의 80 번 포트와 컨테이너의 80 번 포트를 연결.

# 현재 실행중인 컨테이너 확인
PS C:\Users\yangh> docker ps
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS         PORTS                                 NAMES
4119c554f1c5   nginx     "/docker-entrypoint.…"   4 minutes ago   Up 2 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   my-web

# 중지된 컨테이너까지 확인

docker ps -a

CONTAINER ID   IMAGE          COMMAND                   CREATED             STATUS                         PORTS                                 NAMES
   nginx          "/docker-entrypoint.…"   50 seconds ago      Up 48 seconds                  0.0.0.0:80->80/tcp, [::]:80->80/tcp   my-web
   ubuntu:24.04   "/bin/bash"               About an hour ago   Exited (0) About an hour ago                                         cool_bhaskara

```
# 실행중인 컨테이너 중지
```Powershell
docker stop my-web

PS C:\Users\yangh> docker stop my-web
my-web
PS C:\Users\yangh> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                   CREATED          STATUS                     PORTS                  NAMES
4119c554f1c5   nginx                 "/docker-entrypoint.…"   27 seconds ago   Exited (0) 6 seconds ago                          my-web
```

# 중지된 컨테이너 실행
```Powershell
PS C:\Users\yangh> docker start my-web
my-web

PS C:\Users\yangh> docker ps
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS         PORTS                                 NAMES
4119c554f1c5   nginx     "/docker-entrypoint.…"   4 minutes ago   Up 2 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp   my-web
```
**컨테이너와 이미지 삭제 및 제거**

```Powershell
#컨테이너 삭제 (실행중이기 때문에 -f옵션 활용)
docker rm -f my-web

#다운로드 했던 이미지 삭제
docker rmi nginx

#모든 컨테이너 제거
docker rm $(docker ps -aq)

```

# my-web 컨테이너의 로그를 최근 10줄만 확인
```Powershell
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
```

**stats 명령어를 활용해 nginx 컨테이너 리소스 확인**
```Powershell
docker stats 0463441f4828

CONTAINER ID   NAME      CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O     PIDS
0463441f4828   my-web    0.00%     16.96MiB / 7.517GiB   0.22%     1.17kB / 126B   0B / 12.3kB   19

#states -a 옵션을 활용해서 정지된 컨테이너도 확인
docker stats -a 317cfb683799

CONTAINER ID   NAME                 CPU %     MEM USAGE / LIMIT   MEM %     NET I/O   BLOCK I/O   PIDS
317cfb683799   friendly_heyrovsky   0.00%     0B / 0B             0.00%     0B / 0B   0B / 0B     0

```
## 8. 컨테이너 실행 실습

**hello-world 실행 성공을 기록**
```Powershell
# hello-world 실행 성공을 기록
docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

```
**ubuntu 컨테이너 실행 후 수행 결과 기록**
```Powershell
# 우분투 이미지 받고 /bin/bash 쉘로 실행
# -it : 컨테이너 안의 터미널과 내 키보드/화면을 연결해서 상호작용하기 위한 옵션.
# -i : 입력 채널을 열어 키보드의 입력이 -> 터미널로 들어가도록 함
# -t : 가상터미널 배정, 즉 화면 프레임워크(줄바꿈, 색상 지원, root@xxxx:)
docker run -it ubuntu:24.04 /bin/bash
root@xxxx:/#

#ls 명령어 실행 결과
root@xxxx:/# ls
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

#echo 명령어 실행 결과
root@xxxx:/# echo test1
test1

```
**컨테이너 종료/유지(attach/exec 등)의 차이**

**exit(컨테이너 정지)**

```Powershell
docker ps
root@xxxx:/# exit
exit

PS C:\WINDOWS\system32> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

#컨테이너의 main 프로세스(bash)를 종료하니 컨테이너도 함께 정지.
PS C:\WINDOWS\system32> docker ps -a
CONTAINER ID   IMAGE          COMMAND         CREATED          STATUS                      PORTS     NAMES
317cfb683799   ubuntu:24.04   "/bin/bash"     5 minutes ago    Exited (0) 26 seconds ago             friendly_heyrovsky
```
**attach명령어로 실행중인 컨테이너의 메인 화면으로 연결**
```Powershell

PS C:\WINDOWS\system32> docker start 317cfb683799
317cfb683799

PS C:\WINDOWS\system32> docker ps

CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS          PORTS     NAMES
317cfb683799   ubuntu:24.04   "/bin/bash"   9 minutes ago   Up 10 seconds             friendly_heyrovsky

PS C:\WINDOWS\system32> docker attach 317cfb683799
root@317cfb683799:/#
```
**Ctrl + P, Q(컨테이너 유지-Detach)**

**컨테이너는 실행 되는 상태로 내 터미널만 나오기.**
```Powershell
#Ctrl + P ,Q
root@317cfb683799:/# read escape sequence

docker ps
# 컨테이너가 실행 중임을 알 수 있습니다.
PS C:\WINDOWS\system32> docker ps

CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS         PORTS     NAMES
317cfb683799   ubuntu:24.04   "/bin/bash"   12 minutes ago   Up 3 minutes             friendly_heyrovsky
```
**exec로 실행 중인 컨테이너에 들어가기**
**즉, 샐행 중인 컨테이너에 새로운 문을 하나 더 열고 들어가기**
```Powershell

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
선택한 베이스 이미지는 **nginx:latest** 입니다.<br>
**선택 이유**: 가장 가볍고 널리 쓰이는 웹 서버 이미지이며, 정적 파일(HTML)만을 교체할 때 커스텀 **결과를 즉각 확인**하기 좋아서 선택했습니다.

#### 커스텀 포인트 및 목적
파일 교체 (index.html): NGINX의 기본 시작 페이지 대신, 무엇을 위한 페이지인지 알려주기 위함입니다.<br>
포트 포워딩 설정: 호스트(내 컴퓨터)의 8080 포트와 컨테이너의 80 포트를 연결하여 웹 브라우저에서 접속 가능하게 했습니다.

#### 빌드/실행 명령 + 핵심결과

**1. web_base라는 폴더를 만들고 폴더 안에 간단한 정적 index.html 파일을 만듭니다.**
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
**2. 같은 폴더 안에 Dockerfile 을 만들고 다음과 같이 작성해줍니다.**

```Dockerfile
# 1. 베이스 이미지 선택
FROM nginx:latest

# 2. 커스텀 포인트: 내가 만든 index.html을 컨테이너 안의 특정 경로로 복사
# NGINX는 기본적으로 /usr/share/nginx/html 경로의 파일을 웹에 띄워주기에 아래와 같이 작성해줬습니다.
COPY index.html /usr/share/nginx/html/index.html

# 3. (선택) 컨테이너가 80번 포트를 사용함을 명시
EXPOSE 80
```

**3. 이미지 빌드 및 컨테이너 실행**

```shell

# 도커 이미지 빌드
PS C:\web_base> docker build -t web_base:v1 .

[+] Building 1.0s (7/7) FINISHED                                                                                                                                                              docker:desktop-linux


 => => unpacking to docker.io/library/web_base:latest                                                                                                                                                         0.1s
# web_base:v1 . 에서 v1은 기존 이미지인 web_base에 대한 태그 참조를 새로운 태그와 함께 저장.<br>
# .은 현재 경로를 의미 / 즉, 현재 경로에 있어도 . 을 사용하지 않으면 이미지 업로드가 실패합니다!<br>
# 반대로 web_base 경로가 아닌 다른 경로에 있는데 . 을 사용해도 이미지 업로드가 실패 합니다!<br>
# -t 이미지 이름과 태그를 부여해주기 위함입니다.
```

**실패 코드와 오류 내용**
```Powershell

PS C:\code> docker build -t web_base:v1 .
[+] Building 0.2s (1/1) FINISHED                                                                                                                                                              docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                          0.1s
 => => transferring dockerfile: 2B                                                                                                                                                                            0.0s
ERROR: failed to build: failed to solve: failed to read dockerfile: open Dockerfile: no such file or directory

#dockerfile이 없어 찾지 못하니 당연히 실패했던 것 같습니다.

```

**컨테이너 실행**
```Powershell
PS C:\code\web_base> docker run -d -p 8080:80 --name my-web-container web_base:v1
df7e210e37da9352f074de279ff0324c18ff3356c425bf242b1e600a05e5a862
```

#컨테이너 상태가 up 임을 확인했습니다.<br>

**localhost로 들어가서 확인해보니 정상적으로 사이트가 로드되었습니다.**

```Powershell
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

docker build 과정에서 패키지 설치 로그 확인<br>

docker ps를 통해 healthy 상태 및 student 계정 접속 확인<br>

1. linux_base 폴더를 만들고 Dockerfile을 작성해주었습니다


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
#rm -rf /var/lib/apt/lists/* 는 도커 이미지 용량을 줄이기 위해 넣었습니다.
#apt-get update의 메타데이터(apt-get install 명령어를 위해)를 다 사용하고 용량을 줄이기 위해 추가했습니다!

#root 계정으로 로그인시 보안 상 위험 존재(path traversal)
# student라는 사용자명을 만들고 그 명으로 로그인하도록 함.
RUN useradd -m student
USER student

# 작업 디렉터리 설정(로그인 시 바로 이동하여 위치)
WORKDIR /home/student

# 헬스체크 (컨테이너가 주기적으로 잘 작동하는지 확인)
# 30초마다 curl명령어로 구글에 잘 접속되는지 확인
# 구글 주소 이유: 상시 가용성, 인터넷 연결 검증(사내망 넘어 실제 WAN), DNS 정상 작동 확인
HEALTHCHECK --interval=30s --timeout=3s \
 CMD curl -f https://www.google.com || exit 1

# CMD : 컨테이너가 시작될 때 실행할 커맨드를 지정
CMD ["sleep", "3600"]
```


2. Powershell에서 리눅스 기반 도커 이미지를 빌드하고 컨테이너를 실행하였습니다.

```Powershell

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
student@f80cfad02027:~$ echo hello
hello

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


**포트 매핑 필요 이유:**

-외부(호스트/인터넷)에서 격리된 컨테이너 내부의 서비스에 접속할 수 있도록 "외부 포트"와 "내부 포트" 사이에 통로를 뚫어주는 작업!<br>
컨테이너는 기본적으로 독립된 공간!

-호스트와의 포트 충돌 방지및 다중 실행 가능<br>
즉, 하나의 이미지로 여러 개의 독립된 컨테이너를 동시에 구동 가능!


| 항목 | 내용 |
| :--- | :--- |
| **베이스 이미지** | `nginx:latest` |
| **커스텀 목적** | 기본 NGINX 페이지를 사용자 정의 HTML 파일(`src/index.html`)로 교체하여 웹 서비스 배포 |
| **핵심 명령어** | `docker build -t my-web-app:v1 .`<br>`docker run -d -p 8080:80 my-web-app:v1` |
| **포트 매핑** | 호스트 8080번 포트와 컨테이너 80번 포트를 연결하여 외부 접속 허용 |
| **결과** | 브라우저에서 `localhost:8080` 접속 시 커스텀 페이지 정상 출력 확인 |

### Dockerfile 이미지 생성 및 기본 세팅 과정

#### 1. 프로젝트 구조 
-my_web_server/src/index.html<br>
-my_web_server/Dockerfile

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

**포트매핑 증거**
하나의 이미지로 여러 개의 독립된 컨테이너들이 동시에 구동 된 증거
```Powershell
C:\Users\my_web_server> docker ps
CONTAINER ID   IMAGE               COMMAND                   CREATED          STATUS          PORTS                                     NAMES
995056059ad0   my_port_server:v1   "/docker-entrypoint.…"   3 minutes ago    Up 3 minutes    0.0.0.0:8084->80/tcp, [::]:8084->80/tcp   my-port-server-container2
6c8f7bc5490d   my_port_server:v1   "/docker-entrypoint.…"   5 minutes ago    Up 5 minutes    0.0.0.0:8083->80/tcp, [::]:8083->80/tcp   my-port-server-container
```
<img width="913" height="227" alt="Image" src="https://github.com/user-attachments/assets/500003f1-9ccd-46e2-937b-bbe4c78979bd" />

## 바인드 마운트 반영 + 볼륨 영속성 증거


### 1. 개요 및 볼륨 개념
**볼륨(Volume) 사용 이유:** 컨테이너의 파일 시스템은 기본적으로 휘발성이므로, 컨테이너가 삭제되면 내부 데이터도 함께 소멸되기 때문! 
이를 방지하고 **데이터를 컨테이너 생명주기와 독립적으로 분리하여 영구 보존(영속성 확보)**!

#### 도커 볼륨의 3가지 방식
| 방식 | 설명 | 특징 | 예시 |
| :--- | :--- | :--- | :--- |
| **Volume** | Docker가 내부에서 직접 관리 | 영속성 뛰어남, 백업 편리 | 운영 환경의 DB, 앱 데이터 |
| **Bind Mount** | 호스트의 특정 경로를 컨테이너에 직접 등록 | 실시간 반영 가능, 유연성 높음 | 개발 중 코드 공유, 로그 확인 |
| **tmpfs Mount** | 휘발성 메모리(RAM) 공간에 저장 | 고속 처리, 재부팅 시 삭제, Linux 기반 전용 | 캐시, 인증 정보, 민감한 임시 데이터 |

---

### 2. Docker 볼륨 영속성 검증 보고서

#### [실습 개요]
컨테이너 생성 후 볼륨 영역에 데이터를 기록하고, 해당 컨테이너를 완전 삭제한 뒤 새 컨테이너에서 동일 볼륨을 연결하여 데이터 유지 여부를 검증합니다.

#### [검증 절차 및 명령]

**1. 볼륨 생성후 확인**

```shell
docker volume create my_persistence_vol

docker volume ls
DRIVER    VOLUME NAME
local     my_persistence-vol
local     my_persistence_vol
local     my_web_data
local     mydata
```
<img width="280" height="87" alt="Image" src="https://github.com/user-attachments/assets/d9040867-1eb7-44ff-8ff3-c34748fa909c" />

**2. 컨테이너 생성 및 데이터 저장**
생성한 볼륨을 container_1의 /data 경로에 마운트하여 실행하고, 내부에 테스트 파일(test.txt)을 생성했습니다.

```shell
> docker run -it --name container_1 -v my_persistence_vol:/data ubuntu bash
Status: Downloaded newer image for ubuntu:latest
root@e:/# echo "Hello, Docker Volume! This data is persistent." > /data/test.txt
root@e:/# cat /data/test.txt
Hello, Docker Volume! This data is persistent.
```

<img width="671" height="174" alt="Image" src="https://github.com/user-attachments/assets/36dea1f1-7a50-4f41-93da-412c0c28d19b" />

**3. 컨테이너 삭제 및 새 컨테이너에서 데이터 확인**

```shell
>  docker rm container_1   
container_1

> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                   CREATED       STATUS                           PORTS                  NAMES
016057182e6e   ubuntu_nginx_web:v3   "nginx -g 'daemon of…"   4 hours ago   Exited (255) About an hour ago   0.0.0.0:8081->80/tcp   ubuntu_nginx_container
9dee0a323350   my_web_server:v1      "/docker-entrypoint.…"   5 hours ago   Exited (0) 4 hours ago

> docker run -it --name container_2 -v my_persistence_vol:/data ubuntu bash

root@93cf4a0ba852:/# cat /data/test.txt
Hello, Docker Volume! This data is persistent
```

<img width="293" height="31" alt="Image" src="https://github.com/user-attachments/assets/7c1b84bc-b3e2-4a51-8a7e-f34a9c0c707d" />


<img width="676" height="108" alt="Image" src="https://github.com/user-attachments/assets/ae035a3f-31de-4d6a-adae-a43fc0df4379" />


### 바인드 마운트 반영

#### 1. 개요 및 개념
**바인드 마운트란?** 호스트 시스템의 특정 파일이나 디렉토리를 컨테이너 내부 경로로 직접 공유하는 방식입니다.<br>
**주요 특징:** 
   호스트에서 소스 코드를 수정하면 컨테이너 내부에도 **실시간으로 즉시 반영**됩니다.<br>
   개발 환경에서 빌드 과정 없이 코드 변경 사항을 테스트할 때 매우 유용합니다.

#### 2. 바인드 마운트 실습 절차

##### 호스트 디렉토리 및 테스트 파일 생성

호스트에 컨테이너와 연결할 폴더와 HTML 파일 만들기.

**PowerShell / Bash 명령어:**
내 컴퓨터의 C:\Users\yangh\code\my_web_server\src 폴더를 컨테이너에 연결

  ```powershell
docker run -d `
  --name bind-mount-container `
  -p 8082:80 `
  -v "C:\Users\yangh\code\my_web_server\src:/usr/share/nginx/html" `
  nginx  
```
바인드 마운트 확인 페이지!<br>

<img width="337" height="150" alt="Image" src="https://github.com/user-attachments/assets/cc2eb858-3246-4fae-b0db-f74af4310398" />

바인드 마운트 증거<br>

<img width="489" height="108" alt="Image" src="https://github.com/user-attachments/assets/49384033-a474-41bf-86a7-3e0a8e833df1" />

바인드 마운트 성공!<br>
<img width="369" height="180" alt="Image" src="https://github.com/user-attachments/assets/d251cc7a-2a90-49e0-a034-929f5fc1c279" />


## 11. Git 설정 및 GitHub/VSCode 연동 증거

### Git 사용자 정보 및 기본 브랜치 설정결과
```
PS C:\Users\yangh> git config --list
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=openssl
http.sslcainfo=C:/Program Files/Git/mingw64/etc/ssl/certs/ca-bundle.crt
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.name=yanghwan
user.email=jamgyang@gmail.com
core.autocrlf=true
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
init.defaultbranch=main
```
"터미널에서 git config --list를 실행하여 사용자 이름, 이메일 및 기본 브랜치가 main으로 설정된 것을 확인하였습니다."


### Github Repository 생성 및 연동

```shell
PS C:\Users\yangh> cd code
PS C:\Users\yangh\code> git init
Initialized empty Git repository in C:/Users/yangh/code/.git/

PS C:\Users\yangh\code> git remote add origin https://github.com/surilog/codessey.git
PS C:\Users\yangh\code> git remote -v
origin  https://github.com/surilog/codessey.git (fetch)
origin  https://github.com/surilog/codessey.git (push)

PS C:\Users\yangh\code> git add .
PS C:\Users\yangh\code> git commit -m "Initial commit: Docker volume practice"
[main (root-commit) 709e652] Initial commit: Docker volume practice
 4 files changed, 67 insertions(+)
 create mode 100644 linux_base/Dockerfile
 create mode 100644 my_web_server/Dockerfile
 create mode 100644 my_web_server/Dockerfile.ubuntu
 create mode 100644 my_web_server/src/index.html

git pull origin main --allow-unrelated-histories
remote: Enumerating objects: 18, done.
remote: Counting objects: 100% (18/18), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 18 (delta 4), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (18/18), 14.27 KiB | 121.00 KiB/s, done.
From https://github.com/surilog/codessey
 * branch            main       -> FETCH_HEAD
 * [new branch]      main       -> origin/main
Merge made by the 'ort' strategy.
 README.md | 705 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 705 insertions(+)
 create mode 100644 README.md
PS C:\Users\yangh\code> # 2. 다시 푸시
PS C:\Users\yangh\code> git push -u origin main
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 18 threads
Compressing objects: 100% (9/9), done.
Writing objects: 100% (11/11), 2.33 KiB | 238.00 KiB/s, done.
Total 11 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To https://github.com/surilog/codessey.git
   42abdcb..ef5f03f  main -> main
branch 'main' set up to track 'origin/main'.
```
처음에 git push -u origin main을 하엿지만 다음과 같은 오류가 발생했습니다.<br>
이 오류는 찾아보니 Github 페이지에는 있는 파일들이 local에 없어서 생기는 오류여서 github에 있는 파일을 제 local로 가져와 합친 후 다시 올려 해결했습니다.<br>

```powershell
 git push -u origin main
To https://github.com/surilog/codessey.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/surilog/codessey.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
```Powershell
# 1. 원격의 내용을 가져오되, 서로 관련 없는 기록이라도 합치도록 허용
git pull origin main --allow-unrelated-histories

git push -u origin main
```
### Github와 vscode 연동 확인

<img width="880" height="436" alt="Image" src="https://github.com/user-attachments/assets/86e6cbed-1df7-4011-93bd-81f49cb8a43c" />


## 트러블 슈팅

### 1. 포트 충돌 오류

#### 문제상황
새로운 컨테이너를 실행하기 위해 `docker run -d -p 8080:80 ...` 명령어를 입력하였으나, 컨테이너가 생성되지 않고 아래와 같은 포트 바인딩 에러 메시지가 발생함.

```powershell
PS C:\Users\yangh\code\my_web_server> docker run -d -p 8080:80 --name my-port-server-container4 my_port_server:v1
fca635b801235e2d5eec5f4259dae61514e964acf26249c26d9d5a3a8ec0576c

What's next:
    Debug this container error with Gordon → docker ai "help me fix this container error"
docker: Error response from daemon: failed to set up container networking: driver failed programming external connectivity on endpoint my-port-server-container4 (9791005a1437c185f56a61d68e7db411b843035d51744a2273d62b1bd5d2ce1c): Bind for 0.0.0.0:8080 failed: port is already allocated
```
#### 2. 원인 분석 (Cause)
호스트 시스템의 8080번 포트를 이미 다른 프로세스나 이전에 실행한 다른 Docker 컨테이너가 점유하고 있어서 발생하는 포트 충돌 현상이었습니다.

#### 3.해결절차

**방법1. 기존에 8080 포트를 점유 중인 컨테이너 확인 및 중지/삭제**

```powershell
PS C:\Users\yangh\code\my_web_server> docker ps
CONTAINER ID   IMAGE               COMMAND                   CREATED          STATUS          PORTS                                     NAMES
7552f0f1f3b6   my_port_server:v1   "/docker-entrypoint.…"   2 minutes ago    Up 2 minutes    0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-port-server-container3

PS C:\Users\yangh\code\my_web_server> docker rm -f my-port-server-container3
my-port-server-container3

```

**방법2. 호스트의 미사용 포트로 변경하여 실행**

```powershell
PS C:\Users\yangh\code\my_web_server> docker run -d -p 8081:80 --name my-port-server-container5 my_port_server:v1
804cd078856b13611065fdc22782277df9064fb10209dc3e12c469eacf2a5483

PS C:\Users\yangh\code\my_web_server> docker ps
CONTAINER ID   IMAGE               COMMAND                   CREATED          STATUS          PORTS                                     NAMES
804cd078856b   my_port_server:v1   "/docker-entrypoint.…"   3 seconds ago    Up 2 seconds    0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   my-port-server-container5

```
### 2. NGINX 컨테이너 즉시 종료 현상 해결

#### 1. 문제 상황

상황: Ubuntu 베이스 이미지에 NGINX를 설치하고 docker run으로 컨테이너를 실행했으나, 컨테이너가 실행 직후 바로 종료됨 (Exited (0) 상태).<br>
로그 확인: docker ps -a 명령어로 확인 시 컨테이너가 Up 상태가 아닌 종료된 상태로 표시됨.<br>

#### 2. 원인 분석

Docker의 특징: Docker 컨테이너는 내부에 실행 중인 **메인 프로세스**가 종료되면 컨테이너 자체도 종료됨.<br>
NGINX의 동작 방식: NGINX는 기본적으로 '데몬(Daemon)' 모드로 설계되어 실행 시 백그라운드로 숨어버림.<br>
결과: Docker 입장에서는 "어? 실행시킨 프로그램이 끝났네?"라고 판단하여 컨테이너를 자동으로 종료!<br>

#### 3. 해결 방법

Dockerfile 수정: NGINX가 백그라운드로 넘어가지 않고 **포그라운드**에서 계속 실행되도록 설정을 수정했습니다.<br>
명령어: CMD ["nginx", "-g", "daemon off;"]


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


