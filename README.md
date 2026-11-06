# Codyssey 개발 워크스테이션 구축 기술 문서

## 1. 제출 저장소 및 기술 문서

### 프로젝트 개요
로컬 개발 환경 세팅, 재현 가능한 실행 환경 공유, 협업 기반 소스코드 관리를 목표로 리눅스 CLI, Docker, Git/GitHub를 활용한 개발 워크스테이션 구축 및 동작 검증.

### 실행 환경
- **OS**: Windows 11 (WSL 2 Ubuntu 연동)
- **Shell / Terminal**: Windows PowerShell 및 WSL bash
- **Docker**: Docker version 27.2.0, build 3ae425f
- **Git**: [본인의 git --version 결과 기입]

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
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ touch script.sh
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ ls -l script.sh
-rwxrwxrwx 1 poxaci poxaci 0 Nov  6 13:05 script.sh

# 변경 후
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ chmod +x script.sh
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ ls -l script.sh
-rwxrwxrwx 1 poxaci poxaci 0 Nov  6 13:05 script.sh
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ 


```

**디렉토리 권한 변경(1개)**
```Bash
# 변경 전
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ mkdir secure_dir
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ ls -ld secure_dir
drwxrwxrwx 1 poxaci poxaci 512 Nov  6 13:07 secure_dir

# 변경 후
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ chmod 700 secure_dir
poxaci@POXACI:/mnt/c/Users/poxac/Desktop/Codyssey/E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기$ ls -id secure_dir
15199648742403619 secure_dir
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
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run -it --name my-ubuntu bash
Unable to find image 'bash:latest' locally
latest: Pulling from library/bash
522bcfbb914f: Pull complete 
881fc932939e: Pull complete 
57cec69bb0cd: Pull complete 
2e3c11c539ac: Download complete 
0d26fb784f3a: Download complete 
Digest: sha256:bfe8f6d356f2944ec5ad456e44620924862263aa0717036b3c2cdce0ee4c0af0
Status: Downloaded newer image for bash:latest
bash-5.3# echo "Inside Ubuntu"
Inside Ubuntu
bash-5.3# ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
bash-5.3# exit
exit
```

### 컨테이너 종료/유지(attach/exec) 차이 요약
- `docker attach`: 컨테이너의 메인 프로세스(PID 1)에 직접 붙습니다. 터미널을 종료(Ctrl+C)하면 메인 프로세스가 끝나므로 컨테이너도 함께 완전히 종료(Stop)됩니다.

- `docker exec`: 이미 실행 중인 컨테이너 내부에 독립적인 새로운 프로세스(예: bash)를 생성하여 접속합니다. 작업을 마치고 exit로 빠져나와도 원래 컨테이너는 백그라운드에서 계속 유지되며 실행됩니다.

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
+] Building 1.3s (7/7) FINISHED                                                                     docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                 0.0s
 => => transferring dockerfile: 108B                                                                                 0.0s
 => [internal] load metadata for docker.io/library/nginx:alpine                                                      0.2s
 => [internal] load .dockerignore                                                                                    0.0s
 => => transferring context: 2B                                                                                      0.0s
 => [internal] load build context                                                                                    0.0s
 => => transferring context: 89B                                                                                     0.0s
 => [1/2] FROM docker.io/library/nginx:alpine@sha256:785835ea65db4bba6f021acc0f3be91b0fef76a53fe41f8588c6698d2aed06  0.7s
 => => resolve docker.io/library/nginx:alpine@sha256:785835ea65db4bba6f021acc0f3be91b0fef76a53fe41f8588c6698d2aed06  0.0s
 => => extracting sha256:7b229517ed222597277036d94892173207d01a731ab3a88eeb1de2e963a2c788                            0.1s
 => => extracting sha256:12b85cff06fd7d383d36a9c96503fe049c9d4bf3dfda5cbb9ffa6ae99b9c07a6                            0.1s
 => => extracting sha256:657023555a2a19dc6036468198bb350d2c7feaac7f2d0a3901b4dd3cb25e40c5                            0.0s
 => => extracting sha256:7cc330898115ce55acccb0cc1e7dd091df4410c8c5f3c540710ee0ac689f5fcb                            0.0s
 => => extracting sha256:26185d08a3ec318dd85a6c9f2172e714ec13cececd7f33c533b40b1689f9fedb                            0.0s
 => => extracting sha256:2ec5fb117ffa9a78b15fb6a78745ffc569c8da0aee9f36ace4cd67a44b1fef79                            0.0s
 => => extracting sha256:9b0f7dfe35b2711a17f60dadd6d4035dc0cdee152538cc5c0987029e94679548                            0.0s
 => => extracting sha256:b9b388a21ecfde6ac80505f23f527146a2129dfb104e79b029bbd00d4cc1a919                            0.3s
 => [2/2] COPY index.html /usr/share/nginx/html/index.html                                                           0.1s
 => exporting to image                                                                                               0.2s
 => => exporting layers                                                                                              0.1s
 => => exporting manifest sha256:ef96810a14f615de5bb7e83a249bfb12cdd863d308532ef8768355d47a4a51ea                    0.0s
 => => exporting config sha256:218031292b9c8d0fbbe8f321cd47ed908a04836bff8ea348bba41b12cc4eaf25                      0.0s
 => => exporting attestation manifest sha256:abd220b8888338af079812e74c1c132b0b051c0b2928e8351beb09413bb79b3b        0.0s
 => => exporting manifest list sha256:8336ce2fbddfb571343fe1e912baa8fb6ff263fff32c4dcb9f680a03d271b2f4               0.0s
 => => naming to docker.io/library/my-custom-nginx:latest                                                            0.0s
 => => unpacking to docker.io/library/my-custom-nginx:latest  
```
---
## 8. 포트 매핑 및 접속 증거

### 컨테이너 포트 매핑 실행 및 응답
```PowerShell
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> docker run -d -p 8080:80 my-custom-nginx
965efffc75f7faa6fea3e46e00f73b7ff94b8f659aefa989f1b75f6aaa8e3c08
PS C:\Users\poxac\Desktop\Codyssey\E1-1. 내 컴퓨터에 개발자용 '작업실' 만들기> curl.exe http://localhost:8080
<h1>Hello from Codyssey Custom Dockerfile!</h1>
```

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

## Git 설정 및 GitHub 연동

### Git 사용자 정보 설정
```PowerShell
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
credential.https://dev.azure.com.usehttppath=true
init.defaultbranch=master
user.name=poxaci447
user.email=poxaci447@gmail.com
init.defaultbranch=main
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=https://github.com/poxaci447/Codyssey.git
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
