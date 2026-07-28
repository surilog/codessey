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
