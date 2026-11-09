# Codyssey 개발 워크스테이션 구축 기술 문서

## 1. 제출 저장소 및 기술 문서

### 프로젝트 개요
로컬 개발 환경 세팅, 재현 가능한 실행 환경 공유, 협업 기반 소스코드 관리를 목표로 리눅스 CLI, Docker, Git/GitHub를 활용한 개발 워크스테이션 구축 및 동작 검증.

### 실행 환경
- **OS**: Windows 11 (WSL 2 Ubuntu 연동)
- **Shell / Terminal**: Windows PowerShell 및 WSL bash
- **Docker**: Docker version 27.2.0, build 3ae425f
- **Git**: git version 2.56.0.windows.4

### 수행 항목 체크리스트
- [x] 터미널 기본 조작 실습 완료
- [x] 권한 변경 실습 (파일 1개, 디렉토리 1개) 완료
- [x] Docker 설치/데몬 점검 완료
- [x] Docker 기본 운영 명령 수행 완료
- [x] hello-world 및 ubuntu 컨테이너 실행 (attach/exec 비교) 완료
- [x] Dockerfile 기반 커스텀 이미지 제작 완료
- [x] 포트 매핑 및 브라우저/curl 접속 검증 완료
- [x] 순수 Docker 볼륨 영속성 검증 완료
- [x] Git 설정 및 GitHub 연동 완료

### 검증 방법
각 항목별로 터미널(PowerShell/bash)에서 필수 명령어를 직접 실행하고, 그에 대한 콘솔 출력 결과(로그)를 아래 각 목차 코드 블록 내에 기록하여 증명함.

### 디렉토리 구조 및 역할 설명 (재현성 기준)
본 저장소는 로컬 환경에 종속되지 않고 협업자 누구나 동일한 환경을 재현할 수 있도록 다음과 같은 디렉토리 구조 및 역할을 가집니다.

* **`Dockerfile`**: `nginx:alpine`을 베이스로 정적 웹 콘텐츠(`index.html`)를 주입하여 커스텀 이미지를 빌드하는 명세서
* **`index.html`**: 커스텀 웹 서버가 서빙할 메인 정적 웹 페이지 리소스
* **`html_data/`**: 컨테이너 이미지 재빌드 없이 소스코드 수정 사항을 즉시 반영하기 위한 호스트 바인드 마운트 전용 디렉토리
* **`practice/`**: 리눅스/파워쉘 CLI 기본 파일 조작(생성, 복사, 이름변경, 삭제) 실습을 격리하여 진행하기 위한 임시 작업 디렉토리
* **`이미지/`**: 기술 문서 작성 및 제출에 필요한 증빙 스크린샷 보관 디렉토리
* **재현성 기준**: 특정 로컬 절대 경로(`C:\Users\poxac\...`)에 종속되지 않도록 저장소 루트를 기준으로 상대 경로 구조를 유지하여, 타 환경에서 저장소를 클론(`git clone`)하더라도 동일하게 재현되도록 구성함.
---

## 2. 터미널 조작 로그 기록

```powershell
# 현재 위치 확인
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> pwd
Path                                                                      
----                                                                      
C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기

# 폴더 생성 및 이동
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> mkdir practice
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> cd practice

# 빈 파일 생성
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> New-Item empty.txt


    디렉터리: C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기


Mode                 LastWriteTime         Length Name                                                                                   
----                 -------------         ------ ----                                                                                   
-a----      2026-11-06  오후 12:56              0 empty.txt

# 파일 내용 채우기 및 '파일 내용 확인'
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> echo "Hello Codyssey" > test.txt
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> cat test.txt
Hello Codyssey

# 복사, 이동/이름변경
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> cp test.txt copy.txt
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> mv copy.txt rename.txt

# 목록 확인 (숨김 파일 포함)
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> ls -Force

    디렉터리: C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기


Mode                 LastWriteTime         Length Name                                                                                   
----                 -------------         ------ ----                                                                                   
d--h--      2026-11-05  오후 10:04                .git                                                                                   
d-----      2026-11-04   오후 7:05                html_data                                                                              
d-----      2026-11-05  오후 10:12                practice                                                                               
d-----      2026-11-03   오후 9:26                이미지                                                                                 
-a----      2026-11-04   오후 7:11             71 Dockerfile                                                                             
-a----      2026-11-06  오후 12:56              0 empty.txt                                                                              
-a----      2026-11-04   오후 7:11             52 index.html                                                                             
-a----      2026-11-05  오후 10:03          14696 README.md                                                                              
-a----      2026-11-06  오후 12:58             34 rename.txt                                                                             
-a----      2026-11-06  오후 12:58             34 test.txt   

# 삭제
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> rm rename.txt empty.txt
```

---
## 3. 권한 실습 및 증거 기록

(WSL Ubuntu 터미널에서 실행)

**파일 권한 변경 (1개)**
```Bash
# 변경 전
poxaci@POXACI:~$ touch script.sh
poxaci@POXACI:~$ ls -l script.sh
-rw-r--r-- 1 poxaci poxaci 0 Nov  7 21:10 script.sh


# 변경 후
poxaci@POXACI:~$ chmod +x script.sh
poxaci@POXACI:~$ ls -l script.sh
-rwxr-xr-x 1 poxaci poxaci 0 Nov  7 21:10 script.sh


```

**디렉토리 권한 변경(1개)**
```Bash
# 변경 전
poxaci@POXACI:~$ mkdir secure_dir
poxaci@POXACI:~$ ls -ld secure_dir
drwxr-xr-x 2 poxaci poxaci 4096 Nov  7 21:14 secure_dir


# 변경 후
poxaci@POXACI:~$ chmod 700 secure_dir
poxaci@POXACI:~$ ls -ld secure_dir
drwx------ 2 poxaci poxaci 4096 Nov  7 21:14 secure_dir
```
---
## 4. Docker 설치 및 기본 점검

```PowerShell
# 버전 확인
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker --version
Docker version 29.9.2, build a82e528

# docker 데몬의 전반적인 시스템과 환경 설정 정보 출력 : info
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker info
Client:
 Version:    29.9.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.152.0
    Path:     C:\Users\poxac\.docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     C:\Users\poxac\.docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    # 이하 생략
```
---
## 5. Docker 기본 운영 명령 수행

```PowerShell
# 이미지 목록 확인
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker images
                                                                                                     i Info →   U  In Use
IMAGE                             ID             DISK USAGE   CONTENT SIZE   EXTRA
docker/welcome-to-docker:latest   c4d56c24da4f       22.2MB         6.03MB    U   
hello-world:latest                c567b231f656       25.9kB         9.49kB        
my-custom-nginx:latest            b4688ee38f4a       93.6MB         26.3MB        
nginx:alpine                      785835ea65db       94.5MB         27.2MB        
nginx:latest                      5ab5b0b01097        243MB         66.8MB     

# 실행 중인 컨테이너 목록 복사
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker ps
CONTAINER ID   IMAGE                             COMMAND                   CREATED        STATUS        PORTS                                     NAMES
7e08a9a831bd   docker/welcome-to-docker:latest   "/docker-entrypoint.…"   15 hours ago   Up 15 hours   0.0.0.0:8088->80/tcp, [::]:8088->80/tcp   welcome-to-docker

# 전체 컨테이너 목록 복사
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker ps -a
CONTAINER ID   IMAGE                             COMMAND                   CREATED        STATUS        PORTS                                     NAMES
7e08a9a831bd   docker/welcome-to-docker:latest   "/docker-entrypoint.…"   15 hours ago   Up 15 hours   0.0.0.0:8088->80/tcp, [::]:8088->80/tcp   welcome-to-docker

# 운영 로그 및 리소스 확인
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker logs welcome-to-docker
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/11/05 13:05:06 [notice] 1#1: using the "epoll" event method
2026/11/05 13:05:06 [notice] 1#1: nginx/1.29.0
2026/11/05 13:05:06 [notice] 1#1: built by gcc 14.2.0 (Alpine 14.2.0) 
2026/11/05 13:05:06 [notice] 1#1: OS: Linux 6.18.54.1-microsoft-standard-WSL2
2026/11/05 13:05:06 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/11/05 13:05:06 [notice] 1#1: start worker processes
2026/11/05 13:05:06 [notice] 1#1: start worker process 30
# 이하 생략


PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker stats --no-stream
CONTAINER ID   NAME                CPU %     MEM USAGE / LIMIT     MEM %     NET I/O        BLOCK I/O     PIDS
7e08a9a831bd   welcome-to-docker   0.00%     7.082MiB / 7.601GiB   0.09%     1.7kB / 126B   0B / 12.3kB   9
```
---
## 6. 컨테이너 실행 실습

### hello-world 실행
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run --rm hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```
### ubuntu 컨테이너 진입 및 간단 명령
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run -it --name my-ubuntu ubuntu bash
root@19ffecc73e9f:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@19ffecc73e9f:/# echo "hello~" 
hello~
root@19ffecc73e9f:/# exit
exit
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> 
```

### 컨테이너 종료/유지(attach/exec) 차이 요약
- `docker attach`: 컨테이너의 메인 프로세스(PID 1)에 직접 붙습니다. 터미널을 종료(Ctrl+C)하면 메인 프로세스가 끝나므로 컨테이너도 함께 완전히 종료(Stop)됩니다.

- `docker exec`: 이미 실행 중인 컨테이너 내부에 독립적인 새로운 프로세스(예: bash)를 생성하여 접속합니다. 작업을 마치고 exit로 빠져나와도 원래 컨테이너는 백그라운드에서 계속 유지되며 실행됩니다.

---

## 7. 기존 Dockerfile 기반 커스텀 이미지 제작

- 선택한 베이스 방식: (A) 웹 서버 베이스 이미지(NGINX) 활용
- 선택한 베이스 이미지: nginx:alpine
- 커스텀 포인트 목적: 기본 Nginx 시작 페이지 대신, 내가 직접 작성한 커스텀 웹 페이지(index.html)가 호스팅되도록 정적 콘텐츠 교체.

### Dockerfile 내용
```Dokerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

### 빌드 명령 및 핵심 결과
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker build --no-cache -t my-custom-nginx .
[+] Building 0.5s (7/7) FINISHED                                                       docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                  0.0s
 => => transferring dockerfile: 108B                                                                  0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                       0.0s
 => [internal] load .dockerignore                                                                     0.0s
 => => transferring context: 2B                                                                       0.0s
 => [internal] load build context                                                                     0.0s
 => => transferring context: 31B                                                                      0.0s
 => CACHED [1/2] FROM docker.io/library/nginx:alpine@sha256:78583                                     0.1s
 => => resolve docker.io/library/nginx:alpine@sha256:785835ea65db                                     0.1s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                                            0.0s
 => exporting to image                                                                                0.2s
 => => exporting layers                                                                               0.1s
 => => exporting manifest sha256:e1324faa39c93a6a4b5ed26e99415be1                                     0.0s
 => => exporting config sha256:8d653b805beb639558fb7595b17f7755f1                                     0.0s
 => => exporting attestation manifest sha256:25e0c64182a3f05ca869                                     0.0s
 => => exporting manifest list sha256:7bafd5a2669978ef96f43236da9                                     0.0s
 => => naming to docker.io/library/my-custom-nginx:latest                                             0.0s
 => => unpacking to docker.io/library/my-custom-nginx:latest                                          0.0s
```

### Dockerfile 경로 선택 기준 및 재현성 권장 방식
Dockerfile 작성 시 호스트 및 컨테이너 경로를 선택한 기준과 환경 재현성 보장 원칙은 다음과 같습니다.

* **호스트 경로: 상대 경로(`index.html`) 사용 이유**
  * 호스트 소스 파일 경로에 특정 개발자 PC의 절대 경로(`C:\Users\poxac\...`)를 명시하면 다른 개발자 PC나 CI/CD 환경에서 경로 불일치로 빌드가 실패합니다.
  * 빌드 컨텍스트 루트를 기준으로 상대 경로를 지정해야만 어떤 환경에서 저장소를 클론하더라도 빌드 재현성이 100% 보장됩니다.
* **컨테이너 경로: 절대 경로(`/usr/share/nginx/html/index.html`) 사용 이유**
  * 베이스 이미지인 `nginx:alpine` 내부 웹 서버 데몬이 정적 파일을 탐색하도록 사전에 설정된 기본 루트 경로가 `/usr/share/nginx/html/` 절대 경로입니다.
  * 컨테이너 내부는 표준 리눅스 파일 시스템을 따르므로, `WORKDIR`의 변경이나 상대 경로 해석 오류를 방지하고 설정된 목적지에 정확히 파일을 안착시키기 위해 컨테이너 경로는 반드시 절대 경로를 사용해야 합니다.
---
## 8. 포트 매핑 및 접속 증거

### 컨테이너 포트 매핑 실행 및 응답
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run -d -p 8080:80 my-custom-nginx
965efffc75f7faa6fea3e46e00f73b7ff94b8f659aefa989f1b75f6aaa8e3c08
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> curl.exe http://localhost:8080
<h1>Hello from Codyssey Custom Dockerfile!</h1>
```
---

## 9. Docker 볼륨 영속성 검증
```PowerShell
# 볼륨 생성
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker volume create mydata
mydata

# 컨테이너 연결 실행
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run -d -v mydata:/data --name vol-test ubuntu sleep infinity
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
f3f25b84a7d7: Pull complete 
edafa32f5d9e: Pull complete 
21594acc2d8b: Download complete 
Digest: sha256:6a06111c3b0a8600feadb270aec771dc239bf407b25625f93f44ab00e11f4d3c
Status: Downloaded newer image for ubuntu:latest
0e51f33acb8bb50d4ebf6b66bc796465eec10868000fb775eea298df7fadd63c

# 내부에 데이터 기록 후 컨테이너 삭제
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker exec vol-test bash -c "echo 'Persist Test' > /data/test.txt"
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker rm -f vol-test
vol-test

# 컨테이너 삭제 후 새 컨테이너로 연결하여 데이터 유지 증명
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run --rm -v mydata:/data ubuntu cat /data/test.txt
Persist Test
```
### 바인드 마운트 실습 (변경 반영 검증)
호스트 작업 폴더(`html_data`)를 컨테이너 내부 경로(`/usr/share/nginx/html`)에 직접 연결하여, 이미지를 다시 빌드하지 않고도 소스코드 수정 사항이 즉시 웹 서버에 반영됨을 검증함.

```PowerShell
# 1. 호스트 작업 디렉토리에 마운트용 폴더 및 초기 HTML 생성
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> mkdir html_data
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> echo "<h1>Initial Mount</h1>" > html_data/index.html

# 2. 호스트 절대 경로(${PWD}/html_data)를 컨테이너 웹 루트로 바인드 마운트하여 실행 (8082 포트)
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run -d -p 8082:80 -v ${PWD}/html_data:/usr/share/nginx/html --name bind-test nginx:alpine
c8291f09ab31cba41e97423019f6a1e39b7a42129037aef5291cb9a941620c3a

# 3. 초기 상태 접속 검증
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> curl.exe http://localhost:8082
<h1>Initial Mount</h1>

# 4. 이미지 재빌드 없이 호스트 로컬에서 HTML 파일 내용 실시간 수정
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> echo "<h1>Updated without rebuild!</h1>" > html_data/index.html

# 5. 브라우저/curl 재호출 시 즉시 변경 사항이 반영됨을 확인 (변경 반영 입증)
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> curl.exe http://localhost:8082
<h1>Updated without rebuild!</h1>

# 6. 실습 완료 후 바인드 컨테이너 정리
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker rm -f bind-test
bind-test
```

## 10. Git 설정 및 GitHub 연동

### Git 사용자 정보 및 환경 설정 (`git config --list`)
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> git config --list
diff.astextplain.textconv=astextplain
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
http.sslbackend=schannel
core.autocrlf=true
core.fscache=true
core.symlinks=false
pull.rebase=false
credential.helper=manager
credential.[https://dev.azure.com.usehttppath](https://dev.azure.com.usehttppath)=true
init.defaultbranch=master
user.name=poxaci447
user.email=poxaci447@gmail.com
init.defaultbranch=main # <---- main branch
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=[https://github.com/poxaci447/Codyssey.git](https://github.com/poxaci447/Codyssey.git)
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
```

### GitHub 저장소 연동 증거
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> git remote -v
origin  https://github.com/poxaci447/Codyssey.git (fetch)
origin  https://github.com/poxaci447/Codyssey.git (push)
```
- 연동된 GitHub 프로젝트 URL: https://github.com/poxaci447/Codyssey
---

## 11. 트러블슈팅 기록

### 이슈 1: WSL 환경에서 `/mnt/c` 마운트 경로 내 `chmod` 권한 변경 미적용 문제
 
* **문제 상황**:
  * Windows 파티션 경로(`/mnt/c/Users/poxac/...`)에서 `chmod +x script.sh` 및 `chmod 700 secure_dir`를 수행했으나, `ls -l` 확인 결과 권한이 `-rwxrwxrwx`(777)로 고정되어 변하지 않음.
* **원인 가설**:
  * WSL 내부의 `/mnt/c`는 Windows NTFS 파일 시스템이 드라이버(DrvFs)를 통해 가상 마운트된 디렉토리이므로, 리눅스 고유의 POSIX 파일 권한 비트(rwx)가 NTFS 메타데이터에 반영되지 않고 기본 777로 고정 매핑되었을 것이다.
* **검증 (확인)**:
  * 순수 리눅스 네이티브 ext4 가상 디스크 영역인 홈 디렉토리(`~`)로 이동하여 동일 명령을 수행함.
  ```bash
  poxaci@POXACI:~$ cd ~
  poxaci@POXACI:~$ touch test_perm.sh
  poxaci@POXACI:~$ ls -l test_perm.sh
  -rw-r--r-- 1 poxaci poxaci 0 Nov  7 21:10 test_perm.sh
  poxaci@POXACI:~$ chmod 755 test_perm.sh
  poxaci@POXACI:~$ ls -l test_perm.sh
  -rwxr-xr-x 1 poxaci poxaci 0 Nov  7 21:10 test_perm.sh
* 조치 및 해결:
 리눅스 고유의 권한 검증 및 보안 디렉토리 실습은 NTFS 마운트 경로가 아닌 리눅스 네이티브 ext4 영역(~)에서 수행하도록 작업 경로를 분리하여 정상 해결함.
  
### 이슈 2: 동일 이름의 컨테이너 충돌로 인한 실행 실패

* 문제 상황:
 docker run -it --name my-ubuntu ubuntu bash 실행 시 다음과 같은 데몬 에러가 발생하며 컨테이너 구동 실패.
```Plaintext
docker: Error response from daemon: Conflict. The container name "/my-ubuntu" is already in use by container "03e860dc...". You have to remove (or rename) that container to be able to reuse that name.
```
* 원인 가설:
 이전에 테스트 후 exit로 빠져나온 my-ubuntu 컨테이너가 중지(Exited)되었을 뿐 완전히 삭제되지 않고 남아 있어 이름 네임스페이스를 선점하고 있을 것이다.
* 검증 (확인):
 docker ps -a로 중지된 컨테이너 전체 목록을 조회하여 충돌 대상 컨테이너 존재를 확인.
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker ps -a --filter "name=my-ubuntu"
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS                     NAMES
03e860dc03c1   ubuntu    "bash"    5 minutes ago   Exited (0) 4 minutes ago   my-ubuntu
```
* 조치 및 해결:
 종료된 잔여 컨테이너를 강제 삭제한 뒤 정상 재실행함.
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker rm -f my-ubuntu
my-ubuntu
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run -it --name my-ubuntu ubuntu bash
root@19ffecc73e9f:/# exit
```
---