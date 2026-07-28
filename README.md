# 🐳 Windows Docker 개발 워크스테이션 구축

Windows 환경에서 Docker를 활용한 개발 환경을 구축하고,
컨테이너 실행 · 포트 매핑 · 바인드 마운트까지 실습한 프로젝트입니다.

## 📋 프로젝트 개요
- **목표:** Windows에 Docker 개발 환경 구축 및 nginx 웹서버 컨테이너 운영
- **환경:** Windows + WSL2 + Docker Desktop
- **Docker 버전:** `29.6.2`
- **Git 버전:** `2.55.0.windows.3`
- **기간:** 2026년 7월 28일

---

## ✅ 수행 체크리스트
- [ ] 터미널 기본 조작 및 폴더 구성
- [ ] 권한 변경 실습
- [x] Docker 설치/점검
- [ ] hello-world 실행
- [x] Dockerfile 빌드/실행
- [x] 포트 매핑 접속(2회)
- [x] 바인드 마운트 반영
- [ ] 볼륨 영속성
- [x] Git 설정 + VSCode GitHub 연동

---

## 🛠️ 사용 기술
- Docker Desktop (Windows)
- WSL2
- nginx:alpine
- Git Bash

---

## ⚙️ 구축 과정

### 0. 터미널 기본 조작 및 권한 실습
터미널 명령어(디렉토리 이동, 생성) 및 파일 권한(chmod) 변경 결과:
```bash
# 터미널 명령어 (pwd, mkdir, cd, ls 등) 및 chmod 실습 로그 복사 붙여넣기
[여기에 로그 복사]
```

### 1. 사전 준비 및 Docker 점검
- BIOS에서 가상화(Virtualization) 활성화
- WSL2 및 VirtualMachinePlatform 활성화
- Docker 데몬 점검 및 `hello-world` / `ubuntu` 테스트:
```bash
$ docker info
Client:
 Version:    29.6.2
 Context:    desktop-linux
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.111.0
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.27.0
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.35.0-desktop.2
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v5.3.1
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.0.47
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  v0.4.3
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-desktop.exe
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.7
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-dhi.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.31
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-extension.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.0
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-init.exe
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.43.3
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-mcp.exe
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.6
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-model.exe
  offload: Docker Offload (Docker Inc.)
    Version:  v0.6.9
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-offload.exe
  pass: Docker Pass Secrets Manager Plugin (beta) (Docker Inc.)
    Version:  v0.2.0
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-pass.exe
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-sandbox.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.23.1
    Path:     C:\Users\dbral\.docker\cli-plugins\docker-scout.exe

Server:
 Containers: 7
  Running: 3
  Paused: 0
  Stopped: 4
 Images: 5
 Server Version: 29.6.2
 Storage Driver: overlayfs
  driver-type: io.containerd.snapshotter.v1
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Discovered Devices:
  cdi: docker.com/gpu=webgpu
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 nvidia runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: e53c7c1516c3b2bff98eb76f1f4117477e6f4e66
 runc version: v1.3.6-0-g491b69ba
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.18.33.2-microsoft-standard-WSL2
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 8
 Total Memory: 7.527GiB
 Name: docker-desktop
 ID: 4b2df536-bb9a-45b4-a5c0-7eaf92d0fdac
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 HTTP Proxy: http.docker.internal:3128
 HTTPS Proxy: http.docker.internal:3128
 No Proxy: hubproxy.docker.internal
 Labels:
  com.docker.desktop.address=npipe://\\.\pipe\docker_cli
 Experimental: false
 Insecure Registries:
  hubproxy.docker.internal:5555
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Firewall Backend: iptables

# hello-world 실행 로그 복사
[여기에 로그 복사]

# ubuntu 컨테이너 진입 후 ls 또는 echo 실행 로그 복사
[여기에 로그 복사]
```

### 2. 커스텀 이미지 빌드
Dockerfile 작성 (nginx 기반):

```dockerfile
FROM nginx:alpine
LABEL maintainer="이상혁"
ENV APP_NAME=my-web
COPY index.html /usr/share/nginx/html/index.html
```

이미지 빌드:

```bash
docker build -t my-web:1.0 .
```

### 3. 컨테이너 실행 (포트 매핑)
```bash
docker run -d -p 9090:80 --name my-web-9090 my-web:1.0
```
→ 브라우저에서 localhost:9090 접속 시 "Hello Docker! codyssey" 확인 ✅

### 4. 바인드 마운트 실습
호스트 폴더와 컨테이너 폴더를 실시간 연결:

```bash
MSYS_NO_PATHCONV=1 docker run -d -p 9091:80 \
  -v "$(pwd)/mount-test":/usr/share/nginx/html \
  --name bind-test nginx:alpine
```
→ 호스트에서 index.html 수정 시 컨테이너 재시작 없이 즉시 반영 ✅

### 5. Docker 볼륨(Volume) 영속성 검증
볼륨을 생성하고 컨테이너를 삭제해도 데이터가 유지되는지 검증:
```bash
# docker volume create 및 실행/검증 로그 복사
[여기에 로그 복사]
```

### 6. Git 설정 및 연동 증거
Git 기본 설정 및 `git config --list` 확인 결과:
```bash
$ git config --list
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
user.name=이상혁
user.email=dbralfk12@inha.edu
core.repositoryformatversion=0
core.filemode=false
core.bare=false
core.logallrefupdates=true
core.symlinks=false
core.ignorecase=true
remote.origin.url=https://github.com/dbralfk12-stack/github-codyssey-Repository-..git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.main.remote=origin
branch.main.merge=refs/heads/main
```
*(GitHub 및 VSCode 연동 완료)*

---

## 📸 실행 결과

**Git 환경 확인**
![Git Version](images/1.png)

**Nginx 컨테이너 실행 및 바인드 마운트 결과**
| Before (원본/한글 깨짐) | After (인코딩 수정 완료) |
|---|---|
| ![Before](images/5.png) | ![After](images/4.png) |

---

## 🔧 자주 쓴 명령어
| 명령어 | 설명 |
|---|---|
| `docker build -t 이름:태그 .` | 이미지 빌드 |
| `docker run -d -p 호스트:컨테이너 이름` | 컨테이너 실행 |
| `docker ps` | 실행 중인 컨테이너 확인 |
| `docker exec 이름 명령어` | 컨테이너 내부 명령 실행 |
| `docker rm -f 이름` | 컨테이너 강제 삭제 |

---

## 🐛 트러블슈팅
| 문제 | 원인 | 해결 |
|---|---|---|
| 403 권한 에러 | 관리자 권한 부족 | Docker Desktop 관리자 권한 실행 |
| 컨테이너 이름 Conflict | 같은 이름 중복 | `docker rm -f 이름` 후 재실행 |
| Git Bash 경로 변환 오류 | 유닉스 경로를 윈도우 경로로 자동 변환 | 명령 앞에 `MSYS_NO_PATHCONV=1` 추가 |
| 한글 깨짐 () | 인코딩 불일치 | HTML에 `<meta charset="UTF-8">` 추가 |

---

## 💡 배운 점
- 이미지(설계도)와 컨테이너(실행체)의 차이를 이해했다.
- 포트 매핑으로 호스트와 컨테이너를 연결하는 원리를 익혔다.
- 바인드 마운트로 코드 수정이 실시간 반영되는 개발 편의성을 체감했다.
- 에러 메시지를 읽고 원인을 추적하는 디버깅 능력을 길렀다.
- **Docker 볼륨을 활용하여 컨테이너가 삭제되어도 데이터가 안전하게 영속성(Persistence)을 유지함을 확인했다.**
