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
    python3 \
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
CMD ["python3", "-m", "http.server", "8080"]