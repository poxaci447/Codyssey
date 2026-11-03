# Codyssey 개발 환경 구축 및 기초 실습

## 1. 실습 환경
- OS: Windows 11 (PowerShell, WSL 2 - Ubuntu)
- IDE: Visual Studio Code
- 도구: Git, GitHub, Docker Desktop

## 2. 주요 실습 내용 및 명령어

### (1) Git / GitHub 연동
- Git 사용자 설정 및 로컬 저장소 초기화 (`git init`)
- 첫 파일 생성, 스테이징, 커밋 (`git add`, `git commit`)
- GitHub 원격 저장소(`origin`) 연동 및 푸시 (`git push -u origin main`)

### (2) Windows PowerShell CLI 기초
- 경로 확인: `pwd`
- 디렉터리 생성 및 이동: `mkdir practice`, `cd practice`
- 파일 생성 및 내용 확인: `echo ... > test.txt`, `cat test.txt`

### (3) Docker 컨테이너 실습
- WSL 2 및 Virtual Machine Platform 활성화
- Docker Desktop 엔진 구동 확인 (`docker ps`)
- Nginx 웹 서버 컨테이너 구동: `docker run -d -p 8080:80 --name my-web nginx`
- 브라우저 접속 검증 (`http://localhost:8080`)

### (4) Linux (WSL Ubuntu) 환경 실습
- Ubuntu 인스턴스 설정 및 계정 생성
- 셸 스크립트(`run.sh`) 작성
- 파일 권한 확인 및 실행 권한 부여: `chmod +x run.sh`, `ls -l`
- 스크립트 실행 검증: `./run.sh`