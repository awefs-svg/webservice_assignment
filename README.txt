웹서비스설계 한국어 분반 · 13주차 Docker 실습

1. Docker Desktop 또는 Docker Engine을 실행합니다. Linux 컨테이너를 사용합니다.
2. 압축을 해제한 이 폴더에서 터미널을 엽니다.
   Dockerfile과 index.html이 현재 폴더에 있어야 합니다.
3. Week13_Docker_Lab_KO.html 안내를 0번 준비부터 7번 정리까지 순서대로 진행합니다.

- 시작 파일: index.html, Dockerfile, .dockerignore
- 기본 이미지 : nginx1.28-alpine
- Dockerfile은 확장자가 없는 파일입니다. Dockerfile.txt로 바꾸지 마세요.
- HTML의 변경은 실습 6에서 수행합니다. 시작할 때는 v1 상태를 유지합니다.
- 첫 이미지 다운로드에는 인터넷이 필요합니다.

이 자료는 실습 자료입니다. 제출 마감: 2026년 12월 6일(일) 23:59 (한국시간). GitHub 저장소 URL과 실습 보고서 PDF를 제출합니다.

---

v1 이미지 빌드 및 컨테이너 실행

** 현재 제출한 index.html은 실습 06에서 수정한 v2 상태입니다. **

1. v1 소스 준비
: 현재 index.html은 v2 상태이므로 h1 문구를 Release v1 is online으로 수정하고 UTF-8로 저장합니다.

- 수정 전 : Release v2 is online.
- 수정 후 : Release v1 is online.

2. Dockerfile 확인
: Docker Desktop을 실행하고 Dockerfile과 index.html이 있는 폴더에서 터미널을 시작합니다. Dockerfile의 아래의 내용을 확인합니다

- FROM nginx:1.28-alpine
- COPY index.html /usr/share/nginx/html/index.html

3. 기본 이미지 받기 및 v1 이미지 빌드
: 아래의 명령어를 순서대로 실행해 기본 이미지를 받고, v1 이미지를 빌드한 뒤 이미지 목록을 확인합니다.

- docker pull nginx:1.28-alpine
- docker build -t wsd13ko:v1 .
- docker image ls wsd13ko

4. 컨테이너 A 실행
: 생성한 v1 이미지로 컨테이너 A를 실행하고, 실행 상태와 컨테이너 ID를 확인합니다.

- docker run -d --name wsd13ko-a -p 127.0.0.1:18381:80 wsd13ko:v1
- docker ps --filter name=wsd13ko-
- docker inspect --format "{{.Id}}" wsd13ko-a

5. 실행 확인
: 브라우저에서 아래에 작성한 주소에 접속해 v1 웹페이지가 표시되는지 확인합니다. 로컬 index.html 파일을 직접 여는 것과 구분합니다. 

- 컨테이너 A : http://127.0.0.1:18381
- 확인 문구 : Release v1 is online.

---

실습 06 : 소스를 바꾸고 v2 이미지를 만들기

1. 변경 내용
: index.html의 h1 문구만 수정하고 UTF-8로 저장합니다.

- 수정 전 : Release v1 is online.
- 수정 후 : Release v2 is online.

2. v2 이미지 빌드 및 컨테이너 실행
: Docker Desktop을 실행한 뒤 project 폴더에서 제공된 명령어를 실행했습니다.

- docker build -t wsd13ko:v2 .
- docker run -d --name wsd13ko-v2 -p 127.0.0.1:18383:80 wsd13ko:v2
- docker image ls wsd13ko
- docker ps --filter name=wsd13ko-

3. 실습 결과 
: 로컬 HTML을 수정한 뒤 새 이미지를 빌드하고, 그 이미지로 새 컨테이너를 실행해 v2 페이지를 확인했습니다. 기존 A는 자동으로 교체되지 않고 v1 페이지를 유지했습니다.

- 기존 A : http://127.0.0.1:18381/?compare=v1
- 새 v2 컨테이너 : http://127.0.0.1:18383/?compare=v2
