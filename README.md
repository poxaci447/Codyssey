# Codyssey 개발 환경 구축 및 실습 보고서

---
## 1. 프로젝트 및 실습 디렉토리 구조

```text
Codyssey/
├── .git/                 # Git 형상관리 메타데이터 디렉토리
├── html_data/            # 볼륨 마운트 및 데이터 지속성 테스트 디렉토리
│   └── test.txt
├── practice/             # CLI 명령어 실습용 디렉토리
│   └── test.txt
├── Dockerfile            # 커스텀 컨테이너 빌드 명세서
├── index.html            # 컨테이너에 복사할 정적 웹 리소스
└── README.md             # 환경 구축 및 검증 보고서
```
---
## 2. Windows PowerShell CLI 기초 실습

**실행 명령어 및 터미널 출력 로그**

```PowerShell
PS C:\Users\poxac\Desktop\Codyssey> pwd
Path
----
C:\Users\poxac\Desktop\Codyssey

PS C:\Users\poxac\Desktop\Codyssey> mkdir practice
    Directory: C:\Users\poxac\Desktop\Codyssey
Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        2026-11-04   오후 5:40                practice

PS C:\Users\poxac\Desktop\Codyssey> cd practice
PS C:\Users\poxac\Desktop\Codyssey\practice> echo "Hello, Codyssey!" > test.txt
PS C:\Users\poxac\Desktop\Codyssey\practice> cat test.txt
Hello, Codyssey!
PS C:\Users\poxac\Desktop\Codyssey\practice> cd ..
```
---
## 3. Git & GitHub 연동 및 원격 증빙

**원격 저장소 설정 출력**(`git remote -v`)

```PowerShell
PS C:\Users\poxac\Desktop\Codyssey> git remote -v
origin  [https://github.com/poxaci447/Codyssey.git](https://github.com/poxaci447/Codyssey.git) (fetch)
origin  [https://github.com/poxaci447/Codyssey.git](https://github.com/poxaci447/Codyssey.git) (push)
```

**푸시 로그 및 브랜치 상태**
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey> git push -u origin main
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 261 bytes | 261.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
To [https://github.com/poxaci447/Codyssey.git](https://github.com/poxaci447/Codyssey.git)
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```
- GitHub 저장소 링크: https://github.com/poxaci447/Codyssey
---
## 4. 리눅스(WSL 2 Ubuntu) 권한 및 CLI 실습

### 호스트 vs 컨테이너 경로 선택 기준
- **호스트 경로**(C:\..., /mnt/c/...) : 소스 코드 작성, Git 형상관리, 영구 보존이 필요한 데이터 저장시 사용.
- **컨테이너 내부 경로(/usr/share/nginx/html, /data) : 애플리케이션 런타임 파일 격리 실행 시 사용. 컨테이너 종료 시 내부 데이터가 휘발되므로 영속성이 필요한 경우 바인드 마운트 연동 필수.

### 권한 숫자 표기법(Numeric Mode) 원리
- 리눅스 권한은 `r`(읽기=4), `w`(쓰기=2), `x`(실행=1)의 합으로 소유자/그룹/기타 사용자 3자리를 표현함.
- 예 : `chmod 755` -> 소유자: 7(rwx), 그룹: 5(r-x), 기타: 5(r-x)

### `chmod` 전후 `ls -l` 출력 및 스크립트 실행로그

```Bash
poxaci@DESKTOP:~/linux_practice$ echo "echo 'Hello from Linux WSL!'" > run.sh

# 1. chmod 적용 전: 실행 권한 없음 (-rw-r--r--)
poxaci@DESKTOP:~/linux_practice$ ls -l run.sh
-rw-r--r-- 1 poxaci poxaci 31 Nov  4 18:00 run.sh

# 2. 실행 권한 부여 (+x / 755)
poxaci@DESKTOP:~/linux_practice$ chmod +x run.sh

# 3. chmod 적용 후: 실행 권한 부여됨 (-rwxr-xr-x)
poxaci@DESKTOP:~/linux_practice$ ls -l run.sh
-rwxr-xr-x 1 poxaci poxaci 31 Nov  4 18:01 run.sh

# 4. 스크립트 실행 테스트
poxaci@DESKTOP:~/linux_practice$ ./run.sh
Hello from Linux WSL!
```
---
## 5. Docker 핵심 개념 및 네트워크 원리

### 이미지 vs 컨테이너 차이
- **도커 이미지**(Image): 애플리케이션 실행에 필요한 모든 바이너리, 라이브러리, 설정이 불변(Read-Only) 계층으로 패키징된 빌드 설계도.
- **도커 컨테이너**(Container): 이미지를 기반으로 독립된 런타임 공간(Read-Write 계층 추가)에서 실체화되어 격리 실행되는 프로세스 인스턴스.

### 네트워크 네임스페이스 및 포트포워딩 원리
- 도커는 컨테이너마다 독립된 네트워크 네임스페이스(veth, 별도 IP, 가상 라우팅 테이블)를 부여하여 호스트망과 격리함.

- 호스트의 특정 포트(예: 8080)로 들어온 패킷을 컨테이너 내부 포트(80)로 전달하기 위해 iptables 규칙 기반의 **포트포워딩(-p 8080:80) 기술을 사용함.

### 포트 충돌 진단 및 해제 절차

1. 포트 점유 프로세스 확인(Windows):
```PowerShell
netstat -ano | findstr :8080
# Linux/WSL의 경우: ss -ltnp | grep 8080
```
2. PID 확인 후 프로세스 강제 종료:
```PowerShell
taskkill /F /PID <프로세스PID>
# Linux/WSL의 경우: kill -9 <PID>
```

### 데이터 손실 방지를 위한 볼륨/바인드 대안 및 백업 절차
- 컨테이너 삭제 시 내부 데이터가 함께 소멸되므로 호스트 디렉터리를 연결하는 **바인드 마운트(`-v host_path:container_path`) 또는 Docker 볼륨(`docker volume create) 사용을 권장함.
- 백업 절차: 주기적으로 볼륨 경로를 tar 등으로 아카이브(tar -czvf backup.tar.gz /path/to/data)하여 원격 스토리지로 보관함.
---
## 6. Docker 실행 증거 및 빌드/볼륨 검증

### Docker 버전 및 데몬 상태
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey> docker --version
Docker version 27.2.0, build 3ae425f

PS C:\Users\poxac\Desktop\Codyssey> docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                     PORTS                  NAMES
c3d5a1b2c3d4   nginx          "/docker-entrypoint.…"   5 minutes ago   Up 5 minutes               0.0.0.0:8080->80/tcp   my-web

PS C:\Users\poxac\Desktop\Codyssey> docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    51086e40d19b   2 weeks ago    187MB
```

### `hello-world` 실제 실행 로그

```Plaintext
PS C:\Users\poxac\Desktop\Codyssey> docker run --rm hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
d5e71e642bf5: Download complete 
Digest: sha256:c567b231f65614688711a75f31518a064258a51d442117a3e771a3a8fdba78ef
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

### Dockerfile 기반 빌드 증거(`docker build)
- 루트 디렉토리 `Dockerfile` 명세:
```Dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```
- 빌드 성공 로그:
```Plaintext
PS C:\Users\poxac\Desktop\Codyssey> docker build -t my-custom-nginx .
Sending build context to Docker daemon  1.063MB
Step 1/2 : FROM nginx:alpine
 ---> 917342812e0a
Step 2/2 : COPY index.html /usr/share/nginx/html/index.html
 ---> Using cache
 ---> a817293b41ef
Successfully built a817293b41ef
Successfully tagged my-custom-nginx:latest
```

### 호스트 포트 접속 응답(`curl` 검증)

```PowerShell
PS C:\Users\poxac\Desktop\Codyssey> curl.exe http://localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

### 볼륨 마운트 및 컨테이너 삭제 후 데이터 지속성 검증
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey> mkdir -p html_data
PS C:\Users\poxac\Desktop\Codyssey> echo "Volume persist Test" > html_data/test.txt

# 볼륨 바인드 마운트 컨테이너 실행
PS C:\Users\poxac\Desktop\Codyssey> docker run -d -p 8081:80 -v ${PWD}/html_data:/data --name vol-test nginx
da69571656cd0c907cda486bb523f8a7688c2dc8b2c5dc98a112b0dd15300335

# 컨테이너 종료 및 삭제
PS C:\Users\poxac\Desktop\Codyssey> docker stop vol-test
vol-test
PS C:\Users\poxac\Desktop\Codyssey> docker rm vol-test
vol-test

# 컨테이너 삭제 후에도 호스트 디렉터리 내 데이터 영속성 검증 완료
PS C:\Users\poxac\Desktop\Codyssey> cat html_data/test.txt
Volume persist Test
```

## 7. 트러블슈팅 사례 보고서

| 단계 | 발생 사례 1(WSL 가상 머신 플랫폼 비활성화) | 발생 사례 2(PowerShell 파일 인코드 BOM 오류) |
| --- | --- | --- |
| 문제 상황 | Docker Desktop 설치 중 `Virtual Machine Platform Windows feature is not enabled` 오류 발생 | `docker build` 실행 시 `dockerfile parse error in line 1: unknown instruction: FROM` 오류 발생 |
|문제 상황|Docker Desktop 설치 중 `virtual Machine Platfrom Windows feature is not enabled` 오류 발생 | `docker build` 실행 시 `dockerfile parse error on line 1: unknown instruction: FROM` 오류 발생 |
| 가설 설정 | 윈도우 가상화 기능이 꺼져 있어 WSL2 리눅스 백엔드가 초기화되지 못함 | PowerShell의 `echo >` 리디렉션이 파일을 UTF-16 LE(BOM 포함)로 저장하여 데몬이 첫 줄을 잘 못 읽음 |s
| 원인 확인 | DISM 기능 조회 결과 `VirtualMachinePlatform` 기능이 비활성화 상태임을 확인 | 파일 헤더에 바이트 순서 표식(BOM, `0xFF 0xFE`)이 붙어 `FROM` 키워드가 깨짐 확인 |
| 조치 내역 | `dism.exe` 명령어로 VirtualMachinePlatform 기능 활성화 후 윈도우 재부팅 완료| `Set-Content' -path Dockerfile -Value ... -Enconding utf8` 명령어로 BOM 없는 UTF-8 인코딩 파일 재생성 |
| 최종 결과 | Docker Desktop 엔진 정상 구동 및 WSL 2 Ubuntu 인스턴스 부팅 성공 확인 | Dockerfile 빌드 정상 성공(`Successfully tagged my-custom-nginx:latest`) |