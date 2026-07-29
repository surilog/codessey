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



