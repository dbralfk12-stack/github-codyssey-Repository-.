# 🐳 Windows Docker 개발 워크스테이션 구축

Windows 환경에서 Docker를 활용한 개발 환경을 구축하고,
컨테이너 실행 · 포트 매핑 · 바인드 마운트까지 실습한 프로젝트입니다.

## 📋 프로젝트 개요
- **목표:** Windows에 Docker 개발 환경 구축 및 nginx 웹서버 컨테이너 운영
- **환경:** Windows + WSL2 + Docker Desktop
- **Docker 버전:** `[여기에 docker --version 결과 복사]`
- **Git 버전:** `[여기에 git --version 결과 복사]`
- **기간:** 2026년 7월 28일

---

## ✅ 수행 체크리스트
- [ ] 터미널 기본 조작 및 폴더 구성
- [ ] 권한 변경 실습
- [ ] Docker 설치/점검
- [ ] hello-world 실행
- [x] Dockerfile 빌드/실행
- [x] 포트 매핑 접속(2회)
- [x] 바인드 마운트 반영
- [ ] 볼륨 영속성
- [ ] Git 설정 + VSCode GitHub 연동

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
# docker info 결과 복사
[여기에 로그 복사]

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
# git config --list 결과 복사
[여기에 로그 복사]
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
