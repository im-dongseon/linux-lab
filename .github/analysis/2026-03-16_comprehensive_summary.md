# 리눅스 실습 강의 전체 상세 정리

## 문서 개요

이 저장소는 Docker 기반 Ubuntu 24.04 환경에서 리눅스 명령어, `vim`, 파일 처리, 네트워크, 쉘 스크립트, 권한과 프로세스 관리, 그리고 실무형 운영 명령어까지 단계적으로 익히도록 구성된 실습형 강의 자료다.

전체 흐름은 아래처럼 이어진다.

1. 리눅스와 Docker 실습 환경 이해
2. 기본 명령어와 경로 개념 익히기
3. `vi`/`vim` 기본 편집
4. `vim` 심화 설정과 고급 편집
5. 파일/디렉토리/로그/텍스트 처리
6. 네트워크 명령과 기초 쉘 스크립트
7. 쉘 스크립트 심화 문법
8. 권한, 프로세스, 백그라운드 작업, `nohup`, `screen`
9. 실무에서 자주 쓰는 운영 명령어
10. 참고 자료 링크

또한 `qna/` 디렉토리에는 일부 장의 “추가 실습”에 대한 정답 예시가 포함되어 있어, 강의 본문과 답안을 연결해서 복습할 수 있게 되어 있다.

---

## 1. `01_intro.md` — 리눅스란 무엇인가, 실습 환경 만들기

### 핵심 목적

- 리눅스의 기본 철학과 위치를 이해한다.
- Docker Desktop을 사용해 운영체제와 무관하게 동일한 Ubuntu 실습 환경을 만든다.
- 이후 모든 실습을 수행할 컨테이너 기반 실습 구조를 잡는다.

### 주요 내용

#### 1) 리눅스의 개념

- 리눅스는 Unix 계열 운영체제의 한 종류로 소개된다.
- 핵심 키워드는 오픈소스, 무료, 서버 환경의 표준이다.
- 커널은 하드웨어와 소프트웨어를 연결하는 운영체제의 핵심으로 설명된다.
- 배포판 예시로 Ubuntu, CentOS, Debian, Fedora가 나온다.
- GUI보다 CLI 중심 문화가 중요하다는 점을 강조한다.

#### 2) 리눅스가 많이 쓰이는 분야

- 서버: AWS EC2, GCP VM, Naver Cloud VM
- 데이터 엔지니어링: Airflow, Spark, Hadoop
- 머신러닝/AI: 학습 서버, Docker 컨테이너
- 임베디드: Android, Raspberry Pi

#### 3) Docker Desktop 설치

- macOS와 Windows 환경 각각에 대해 Docker Desktop 설치 방법을 소개한다.
- Windows는 `wsl --install` 후 WSL2 기반 엔진 사용을 안내한다.
- Docker가 켜져 있어야 컨테이너 명령이 동작한다는 점을 명시한다.

#### 4) Ubuntu 컨테이너 실행

```bash
docker run -it --name linux-lab ubuntu:24.04
```

- `-it`: 인터랙티브 터미널 연결
- `--name linux-lab`: 컨테이너 이름 지정
- `ubuntu:24.04`: Ubuntu 24.04 LTS 이미지 사용

프롬프트가 `root@...:/#` 형태로 바뀌면 실습 환경 진입 성공이다.

#### 5) 필수 패키지 설치

```bash
apt update && apt install -y vim curl wget net-tools tree htop sudo
```

- 실습 전반에 필요한 도구들을 한 번에 설치한다.
- `tzdata` 설정 화면이 뜰 수 있고, 예시로 Asia/Seoul 선택 절차도 안내한다.

#### 6) 컨테이너 관리

- `docker ps`: 실행 중 컨테이너 확인
- `docker ps -a`: 중지된 것 포함 전체 확인
- `docker stop linux-lab`: 중지
- `docker start -ai linux-lab`: 재접속
- `docker rm linux-lab`: 삭제

#### 7) 로컬 폴더 공유

```bash
mkdir -p ~/workspace/docker/linux-lab
docker run -it -v ~/workspace/docker/linux-lab:/root/workspace --name linux-lab ubuntu:24.04
```

- 로컬 디렉토리를 `/root/workspace`에 마운트해 컨테이너와 파일을 공유한다.
- 기존에 같은 이름의 컨테이너가 있으면 `docker start -ai linux-lab` 또는 `docker rm -f linux-lab`로 처리한다.

#### 8) 접속 단축 alias

```bash
echo "alias linuxlab='docker start -ai linux-lab'" >> ~/.zshrc
source ~/.zshrc
```

- 이후 `linuxlab`만 입력해 컨테이너에 재접속할 수 있게 한다.

#### 9) 실습 자료 구조

- `~/workspace/repo/linux-lab/` 아래에 강의 저장소를 두고
- `~/workspace/docker/linux-lab/`를 컨테이너와 동기화되는 작업 폴더로 사용한다.

### 이 장의 의미

이 문서는 “리눅스 자체”보다 “실습을 시작할 수 있는 표준 환경”을 먼저 맞추는 데 초점이 있다. 이후 장들은 모두 이 컨테이너 환경을 바탕으로 진행된다.

---

## 2. `02_basic.md` — 기본 명령어 실습

### 핵심 목적

- Docker 컨테이너 안에서 가장 기초적인 명령어 사용법을 익힌다.
- 경로 이동, 현재 위치 확인, 파일 목록 보기, 파일 생성/출력 같은 CLI 감각을 익힌다.

### 주요 실습

#### 1) 디렉토리 이동

```bash
cd /root/workspace
```

- 이후 대부분의 실습은 공유 디렉토리 기준으로 진행된다.

#### 2) 현재 경로 확인

```bash
pwd
```

- 기대 결과는 `/root/workspace`

#### 3) 파일 목록 보기

```bash
ls
ls -lh
```

- `ls`: 기본 목록
- `ls -lh`: 사람이 읽기 쉬운 크기 단위 포함

#### 4) 디렉토리 트리 보기

```bash
apt update && apt install -y tree
tree
```

- 계층 구조를 시각적으로 파악한다.
- `/`에서 `tree`를 실행하면 출력이 길어질 수 있으므로 `Ctrl+C`로 중지하는 법도 안내한다.

#### 5) 파일 생성

```bash
touch test.txt
```

- 로컬 마운트 경로에도 동일 파일이 생기는 점을 확인하게 한다.

#### 6) 파일 내용 출력

```bash
echo "Hello Linux" > test.txt
cat test.txt
```

- 리다이렉션 `>`로 파일 내용을 쓴다.

#### 7) 상대경로와 절대경로

- `cd ..`: 상위 디렉토리로 이동
- `cd workspace`: 상대경로로 재진입
- `cd -`: 직전 디렉토리로 복귀

#### 8) 도움말 확인

```bash
sudo unminimize
sudo apt install -y man-db manpages
man ls
ls --help
```

- `man`: 공식 매뉴얼, 매우 상세
- `--help`: 빠른 요약형 도움말

#### 9) 종료와 재접속

- `exit`: 컨테이너 셸 종료
- `docker start -ai linux-lab`: 다시 접속

### 추가 실습과 정답

과제는 `/root/workspace`에서 `"Hello World"`를 담은 `hello.txt`를 만드는 것이다.

정답 파일 `qna/02_basic_answer.md`의 해법:

```bash
cd /root/workspace
echo "Hello World" > hello.txt
cat hello.txt
```

### 이 장의 의미

이 장은 CLI 초심자에게 가장 중요한 “경로 + 파일 + 출력” 감각을 익히게 한다. 뒤 장에서 나오는 거의 모든 명령이 여기의 흐름 위에 쌓인다.

---

## 3. `03_vim_basic.md` — vi/vim 기본

### 핵심 목적

- `vim`에서 파일을 열고, 수정하고, 저장하고, 종료하는 기본 순서를 익힌다.
- 모드 기반 편집기라는 개념을 이해한다.

### 주요 내용

#### 1) vi와 vim 소개

- `vi`: 전통적인 유닉스 텍스트 에디터
- `vim`: `vi`의 개선판으로 구문 강조, 다중 undo 같은 기능 제공

#### 2) 세 가지 기본 모드

- NORMAL: 이동/복사/삭제 명령 수행
- INSERT: 실제 텍스트 입력
- COMMAND: `:`로 시작하는 저장/종료/검색 명령 수행

#### 3) 핵심 키

이동:

- `h`, `j`, `k`, `l`
- `w`, `b`
- `0`, `$`
- `gg`, `G`

편집:

- `i`, `a`, `o`
- `dd`, `yy`, `p`
- `u`, `Ctrl+r`

저장과 종료:

- `:w`
- `:q`
- `:wq`
- `:q!`
- `ZZ`

검색:

- `/단어`
- `n`, `N`

### 주요 실습

#### 1) 새 파일 생성 후 저장

```bash
apt update && apt install -y vim
vim note.txt
```

- `i`로 입력 모드 진입
- 내용 입력
- `Esc`
- `:wq`
- 이후 `cat note.txt`로 확인

#### 2) 기존 파일 수정

- `vim test.txt`
- `G`로 파일 끝 이동
- `o`로 새 줄 입력
- `"edited by vim"` 추가
- `:wq`

#### 3) 검색

- `vim note.txt`
- `/vim`
- `n`
- `:q!`

### 추가 실습과 정답

과제는 `note.txt`에 3줄을 추가하고 2번째 줄을 `dd`로 삭제하는 것이다.

정답 파일 `qna/03_vim_basic_answer.md`에서는 다음 흐름을 제시한다.

- `vim note.txt`
- `G`로 파일 끝 이동
- `o`로 새 줄 3개 입력
- `gg`로 파일 시작 이동
- `2G` 또는 `j`로 2번째 줄로 이동
- `dd`
- `:wq`
- `cat note.txt`

### 이 장의 의미

이 장은 리눅스 서버 환경에서 GUI 에디터 없이도 파일을 다룰 수 있도록 만드는 입문 단계다.

---

## 4. `04_vim_advanced.md` — vi/vim 심화

### 핵심 목적

- 실무형 `vim` 사용법을 익힌다.
- `.vimrc` 설정, Visual 모드, 블록 편집, 쉘 스크립트 작성에 유용한 기능을 배운다.

### 주요 내용

#### 1) `.vimrc`의 필요성

- 기본 설정이 미니멀한 `vim`을 실사용 가능한 환경으로 바꾼다.
- 홈 디렉토리의 `~/.vimrc`에 저장해야 자동 적용된다.
- 공유 볼륨이나 현재 경로에 `.vimrc`를 두면 제대로 적용되지 않을 수 있음을 강조한다.

#### 2) 권장 `.vimrc` 설정

대표 옵션:

- `set number`
- `set relativenumber`
- `set list`
- `set listchars=eol:↵,tab:»·,trail:·`
- `set encoding=utf-8`
- `set tabstop=4`
- `set expandtab`
- `set autoindent`
- `set smartindent`
- `set hlsearch`
- `set incsearch`
- `syntax on`

문서에서는 실제 입력 예시로 다음 최소 설정을 제시한다.

```vim
set number
set tabstop=4
set expandtab
set autoindent
set hlsearch
syntax on
set encoding=utf-8
set list
set listchars=eol:↵,tab:»·,trail:·
```

#### 3) Visual 모드

- `v`: 문자 단위 선택
- `V`: 줄 단위 선택
- `Ctrl+v`: 직사각형 블록 선택

활용 예:

- `V` + `j` + `y` + `p`: 여러 줄 복사/붙여넣기
- `3yy`, `p`, `P`: 줄 수 기반 복사/붙여넣기
- `Ctrl+v` 후 `I`, `#`, `Esc`: 여러 줄 앞에 주석 문자 추가

#### 4) 고급 이동

- `:10`: 특정 줄 이동
- `Ctrl+d`, `Ctrl+u`
- `Ctrl+f`, `Ctrl+b`
- `%`: 괄호 짝 이동

#### 5) 쉘 스크립트 작성에 유용한 기능

- `:set filetype=sh`
- `>>`, `<<`
- `=G`
- `:!bash %`
- `Ctrl+n`
- `:syntax on`

### 추가 실습과 정답

`qna/04_vim_advanced_answer.md`에는 두 가지 정답이 있다.

#### 1) `.vimrc` 설정 후 쉘 스크립트 실행

- `vim ~/.vimrc`로 설정 저장
- `vim hello.sh`로 스크립트 작성
- `:!bash %`로 현재 파일 바로 실행

#### 2) visual block으로 3줄 주석 처리

- `Ctrl+v`
- `j` 두 번
- `I`
- `#`
- `Esc`

### 이 장의 의미

단순 편집에서 끝나지 않고, `vim`을 “코드 작성 도구”로 끌어올리는 장이다. 이후 쉘 스크립트 실습과 자연스럽게 연결된다.

---

## 5. `05_file_dir.md` — 파일·디렉토리 다루기

### 핵심 목적

- 파일 생성, 복사, 이동, 삭제, 검색, 출력, 필터링, 파이프를 익힌다.
- 실제 로그 형태의 파일을 만들어 텍스트 처리 감각을 익힌다.

### 주요 내용

#### 1) 로그 파일 만들기

두 종류의 로그 예시를 만든다.

- `sample.log`: 100줄짜리 단순 INFO 로그
- `system.log`: `INFO`, `WARN`, `ERROR`가 섞인 150줄짜리 로그

이를 통해 `head`, `tail`, `grep` 실습에 현실감을 준다.

#### 2) 파일 복사

```bash
cp system.log backup.txt
cp ./backup.txt src_folder/
cp -r src_folder dst_folder
```

- 일반 파일 복사와 디렉토리 복사 차이를 `-r` 옵션 중심으로 설명한다.

#### 3) 이동 및 이름 변경

```bash
mv backup.txt newname.txt
mv newname.txt /root/workspace/logs/
```

- 대상 디렉토리가 없을 때 오류가 나는 경험을 일부러 보여주고 `mkdir`로 해결하게 한다.

#### 4) 삭제

```bash
rm file.txt
rm -r dst_folder
rm -rf src_folder
```

- `rm`은 휴지통이 없고 즉시 삭제된다고 설명한다.
- 특히 `rm -rf /`는 절대 입력하면 안 되는 위험한 명령으로 강하게 경고한다.

#### 5) 검색

```bash
find . -name "*.txt"
find /root/workspace -type f -name "error*"
```

- 이름 패턴 기반 파일 검색

#### 6) 파일 내용 보기

- `cat system.log`
- `head system.log`
- `tail system.log`
- `less system.log`

또한 `big.log`를 만들어 큰 파일 앞/뒤 일부만 보는 실습도 제공한다.

#### 7) 파일 합치기

```bash
echo "hello" > a.txt
echo "world" > b.txt
cat a.txt b.txt > all.txt
```

#### 8) `grep`

```bash
grep "ERROR" system.log
grep -i "error" system.log
grep -ril "error" /root/workspace
```

- 대소문자 무시
- 재귀 검색
- 파일 이름만 출력

#### 9) 파이프

```bash
cat system.log | grep -i "error"
grep "WARN" system.log | head
```

- 앞 명령의 출력을 뒤 명령의 입력으로 넘기는 리눅스식 조합 철학을 보여준다.

### 추가 실습과 정답

`qna/05_file_dir_answer.md`는 두 문제에 대한 정답을 제공한다.

#### 1) `ERROR` 줄만 저장

```bash
mkdir log
grep "^ERROR" system.log > log/error_summary.txt
cat log/error_summary.txt
```

- `^ERROR`를 사용해 “ERROR로 시작하는 줄”만 추출한다.

#### 2) `hello`와 `world` 파일 병합

```bash
echo "hello" > a.txt
echo "world" > b.txt
cat a.txt b.txt > all.txt
```

### 이 장의 의미

이 장은 리눅스의 파일 조작 기본기를 실전적으로 다룬다. 특히 로그 파일을 만들어 `grep`, `head`, `tail`, 파이프의 필요성을 자연스럽게 이해시키는 구조가 좋다.

---

## 6. `06_network_script.md` — 네트워크와 쉘 스크립트 기초

### 핵심 목적

- 네트워크 명령어와 HTTP 요청 도구를 익힌다.
- 환경 변수와 기초 쉘 스크립트 작성법을 배운다.
- 반복문, 조건문, 시스템 정보 조회로 자동화의 첫 단계를 경험한다.

### 주요 내용

#### 1) 네트워크 연결 확인

```bash
apt update && apt install -y iputils-ping curl
ping -c 4 google.com
```

- Ubuntu 최소 이미지에는 `ping`이 없을 수 있어 설치부터 안내한다.

#### 2) `curl` 기본 사용

```bash
curl https://example.com
curl ifconfig.me
```

- HTML 조회
- 공인 IP 확인

테스트 사이트로 `httpbin.org`, `postman-echo.com`, `ifconfig.me`, `example.com`을 소개한다.

#### 3) `curl` 옵션

- `-v`: 상세 로그
- `-I`: 헤더만 보기
- `-L`: 리다이렉트 따라가기
- `-H` + `-d`: 헤더 추가와 JSON POST

예:

```bash
curl -v -X POST https://httpbin.org/post -H "Content-Type: application/json" -d '{"name":"linux-user"}'
```

#### 4) 다운로드

```bash
curl -o index.html https://www.example.com/
curl -O https://www.example.com/index.html
wget https://www.example.com/
```

- `-o`: 저장 파일명 지정
- `-O`: 원래 파일명 사용

#### 5) 환경 변수

```bash
echo $PATH
export MYNAME="linux-user"
echo $MYNAME
echo 'export MYNAME="linux-user"' >> ~/.bashrc
```

- `export`만 하면 세션 종료 시 사라지므로 `.bashrc`에 저장해야 유지된다.
- `.bash_profile`과 `.bashrc`의 차이도 설명한다.

#### 6) Hello World 스크립트

```bash
cat << 'EOF' > hello.sh
#!/bin/bash
echo "Hello World from Shell Script"
EOF
chmod +x hello.sh
./hello.sh
```

- shebang, 실행 권한, 직접 실행 흐름을 보여준다.

#### 7) 날짜 기반 로그 스크립트

- `date +"%Y%m%d"`를 이용해 날짜가 들어간 파일명 생성
- 자동 로그 파일 생성 개념 학습

#### 8) `for`

```bash
for i in {1..3}; do
  echo "File $i" > file$i.txt
done
```

#### 9) `if`

디스크 사용량을 읽어 70% 초과 시 경고하는 `check_disk.sh` 예제를 제공한다.

핵심 흐름:

- `df -h /`
- `awk '{print $5}'`
- `sed 's/%//'`
- 숫자 비교 후 메시지 분기

#### 10) 시스템 정보 조회

- `top`
- `free -h`
- `df -h`
- `ps aux`

### 추가 실습과 정답

`qna/06_network_script_answer.md`에서는 공인 IP를 파일로 저장하는 `get_ip.sh` 예제를 제시한다.

```bash
cat << 'EOF' > get_ip.sh
#!/bin/bash
curl ifconfig.me > my_ip.txt
echo "IP saved to my_ip.txt"
EOF

chmod +x get_ip.sh
./get_ip.sh
```

### 이 장의 의미

이 문서는 “리눅스 명령 사용”에서 “자동화 가능한 작은 프로그램 작성”으로 넘어가는 연결 다리 역할을 한다.

---

## 7. `07_script_advanced.md` — 쉘 스크립트 심화

### 핵심 목적

- 쉘 스크립트의 주요 문법 요소를 체계적으로 익힌다.
- 함수, 변수, 인자, 선택문, 반복문, 디버깅을 학습한다.

### 주요 내용

#### 1) 사용자 변수

예제 `test_var.sh`:

- `GREETING="Hello, Shell Script!"`
- `COUNT=5`
- `echo $GREETING`
- `echo "We have $COUNT apples."`

핵심은 변수 선언 시 공백 없이 작성하고, 사용 시 `$`를 붙인다는 점이다.

#### 2) 환경 변수와 예약 변수

예제 `test_env.sh`:

- `$USER`
- `$PWD`
- `$HOME`
- `$RANDOM`

쉘이 기본 제공하는 값들을 확인하게 한다.

#### 3) 위치 매개변수

예제 `calc.sh`:

- `$0`: 스크립트 이름
- `$1`, `$2`: 첫 번째, 두 번째 인자
- `$#`: 전체 인자 개수
- `SUM=$(($1 + $2))`: 간단한 산술 연산

#### 4) `case`

예제 `select_menu.sh`:

- 사용자에게 `start/stop/restart`를 입력받고
- `case $ACTION in ... esac`으로 분기 처리

`if-elif`보다 명확한 다중 분기 구조를 보여준다.

#### 5) `while`

예제 `while_test.sh`:

- `NUM=1`
- `[ $NUM -le 3 ]`
- `NUM=$((NUM + 1))`

조건이 참인 동안 반복하는 구조를 학습한다.

#### 6) 함수

예제 `functions.sh`:

```bash
print_info() {
  echo "[INFO] $1"
}
```

- 함수 정의
- 인자 전달
- 재사용

#### 7) 디버깅

```bash
bash -x while_test.sh
```

- 각 줄이 어떻게 실행되는지 추적할 수 있다.

### 추가 실습과 정답

`qna/07_script_advanced_answer.md`는 `create_users.sh` 예시를 제공한다.

핵심 요소:

- `add_user()` 함수 정의
- 인자 개수 확인: `if [ $# -ne 2 ]`
- `$1`, `$2`를 함수에 전달
- 사용법 오류 시 `exit 1`

이 정답은 단순 출력뿐 아니라 기본적인 예외 처리까지 포함하고 있다.

### 이 장의 의미

이 장에서부터는 단발성 명령 실행이 아니라 “재사용 가능한 스크립트 구조”를 생각하게 만든다.

---

## 8. `08_permission_process.md` — 권한, 프로세스, 백그라운드, `nohup`, `screen`

### 핵심 목적

- 파일 권한과 소유권을 이해한다.
- 프로세스 상태를 읽고 종료하는 법을 익힌다.
- 백그라운드 실행, 세션 종료 후 유지, 가상 터미널 세션 관리까지 배운다.

### 주요 내용

#### 1) 권한 확인과 변경

- `ls -l`: 권한 확인
- `chmod +x script.sh`
- `chmod 755 script.sh`
- `chown root:root test.txt`

`script.sh`와 `test.txt`를 미리 만들어 두고 실습한다.

#### 2) 프로세스 확인

- `ps aux`
- `ps -ef | grep bash`
- 컬럼 의미 설명: `USER`, `PID`, `%CPU`, `%MEM`, `PPID`, `TTY`, `CMD` 등

단순히 명령만 제시하지 않고 출력 컬럼 해석까지 설명한다.

#### 3) 백그라운드 실행

```bash
sleep 30 &
jobs
ps -ef | grep sleep
```

- `&`로 셸을 점유하지 않게 실행
- `jobs`와 `ps` 두 관점에서 확인

#### 4) 프로세스 종료

```bash
kill <PID>
kill -9 <PID>
```

- 일반 종료를 먼저 시도하고
- `kill -9`는 데이터 손실 위험 때문에 남용하지 말라고 경고한다.

#### 5) 로그 작성 스크립트와 실시간 모니터링

`log_writer.sh`:

- 무한 루프
- 현재 시간과 메시지를 `system_loop.log`에 5초마다 추가

실행 후:

- `./log_writer.sh &`
- `tail -f system_loop.log`

#### 6) `nohup`

```bash
nohup ./log_writer.sh &
```

- 터미널 종료 후에도 프로세스를 살려두는 방식
- 기본 출력이 `nohup.out`으로 간다는 점 설명
- `> mylog.log 2>&1 &` 리다이렉션 조합도 자세히 설명한다.

#### 7) `screen`

- 설치: `apt update && apt install -y screen`
- 생성: `screen -S mysession`
- 분리: `Ctrl + A, D`
- 목록: `screen -ls`
- 재접속: `screen -r mysession`
- 종료: 세션 내부 `exit`

#### 8) `screen` × `ps` × `tail` 연계

- `screen` 안에서 장시간 스크립트 실행
- 빠져나와서 `tail -f`로 로그 확인
- 다시 접속 후 `ps`와 `kill`로 정리

#### 9) 비교표

문서가 특히 잘 정리한 부분:

- `screen` vs `nohup`
- `command &` vs `nohup command &`

즉, 단순 백그라운드 실행과 “세션 종료 후 유지”는 다르다는 점을 분명히 구분한다.

### 추가 실습과 정답

`qna/08_permission_process_answer.md`는 세 문제의 해답을 제공한다.

#### 1) `sleep 100` 실행 후 종료

```bash
sleep 100 &
ps -ef | grep sleep
kill <PID>
```

#### 2) `nohup` 유지 확인

```bash
nohup ./log_writer.sh &
exit
ps -ef | grep log_writer
tail -f nohup.out
```

#### 3) `screen` detach/reattach

```bash
screen -S logsession
./log_writer.sh
screen -ls
screen -r logsession
ps -ef | grep log_writer
kill <PID>
```

### 이 장의 의미

이 장은 실습 난도가 실무에 가장 가깝다. “프로세스가 왜 살아 있나/죽었나”, “터미널을 닫아도 계속 돌게 하려면 무엇을 써야 하나” 같은 운영 감각을 길러준다.

---

## 9. `09_practical_cmd.md` — 실무에서 자주 쓰는 명령어

### 핵심 목적

- 서버 운영, 배포, 장애 대응에서 자주 만나는 명령어를 한 번에 다룬다.
- 용량, 압축, 위치 추적, 로그 열람, 텍스트 처리, 히스토리, 서비스 운영까지 실무형 관점을 제공한다.

### 주요 내용

#### 1) 용량 확인

- `du -sh .`
- `du -sh *`
- `du -sh logs/*`
- `df -h`
- `df -h /`

어떤 폴더가 공간을 많이 쓰는지, 전체 디스크 상태는 어떤지 확인하는 용도다.

#### 2) 압축과 해제

`myapp` 테스트 디렉토리를 만들고:

- `tar -czvf myapp.tar.gz myapp/`
- `tar -xzvf myapp.tar.gz`
- `zip -r myapp.zip myapp/`
- `unzip myapp.zip`

를 실습한다.

#### 3) 명령어 위치 찾기

- `which python3`
- `which curl`
- `whereis python3`
- `whereis nginx`

`which`는 실제 실행 파일, `whereis`는 관련 파일과 man 페이지까지 찾는 차이를 설명한다.

#### 4) 로그 보기 도구 비교

- `tail`: 파일 끝부분 빠르게 확인
- `less`: 위아래 이동, 페이지 단위 탐색, 검색
- `view`: 읽기 전용 `vim`

문서는 `tail`과 `less`의 용도 차이를 명확하게 정리한다.

#### 5) `grep` 심화

테스트용 `logs2/`를 만들고:

- `grep -n "Error" logs2/app.log`
- `grep -i "error" logs2/app.log`
- `grep -rn "INFO" logs2`

를 실습한다.

#### 6) `cut`

`users.csv`를 만들고:

```bash
cut -d',' -f1,3 users.csv
```

- 쉼표 구분 데이터에서 특정 컬럼만 선택하는 법을 익힌다.

#### 7) `awk`

```bash
awk -F',' '{print $1, $2}' users.csv
awk -F',' 'NR>1 && $3 >= 30 {print $2, $3}' users.csv
```

- 단순 문자열 검색을 넘어 컬럼 기반 조건 처리를 다룬다.

#### 8) 히스토리 활용

- `history`
- `history | grep tar`
- `!!`
- `!25`

긴 명령을 반복 실행할 때의 생산성 향상을 보여준다.

#### 9) `uptime`

- 서버 가동 시간
- 사용자 수
- 최근 부하 평균(load average)

를 한 줄로 빠르게 확인하는 명령으로 소개한다.

#### 10) 서비스 관리와 nginx

Docker Ubuntu 환경에서는 `systemctl` 대신 `service nginx`를 사용한다는 점을 안내한다.

- `service nginx status`
- `service nginx start`
- `service nginx stop`
- `service nginx restart`

#### 11) Docker에서 nginx 띄우기

문서 후반은 “서버형 프로세스”를 컨테이너로 실행하는 사례를 자세히 다룬다.

##### 방법 A: Ubuntu 컨테이너 안에 nginx 설치

```bash
docker run -it -p 8080:80 --name ubuntu-nginx ubuntu:24.04
apt update && apt install -y nginx
nginx
ps aux | grep nginx
curl localhost:8080
```

##### 방법 B: 공식 nginx 이미지 사용

```bash
docker run -d -p 8080:80 --name my-nginx nginx
docker logs -f my-nginx
docker exec -it my-nginx bash
docker stop my-nginx
docker rm my-nginx
```

또한 `-d` 옵션을 왜 써야 하는지 별도 표로 설명한다.

### 추가 실습과 정답

`qna/09_practical_cmd_answer.md`는 종합 미션 전체의 정답을 제공한다.

- `df -h`, `du -sh * | sort -h | tail -3`
- tar.gz/zip 압축과 해제
- `cut -d',' -f2,3 users.csv`
- `awk -F',' 'NR>1 {print $2, $3}' users.csv`
- `grep -rni "error" logs2`
- `view users.csv`, `less big.log`, `uptime`

즉, 이 장은 앞서 배운 여러 명령을 실무 문맥으로 묶어 재활용하는 성격이 강하다.

### 이 장의 의미

강의 전체에서 가장 “운영 업무”에 가까운 장이다. 시스템 상태 점검, 로그 조회, 압축 전달, 명령 위치 확인, 컨테이너 서비스 실행까지 넓게 다룬다.

---

## 10. `10_reference.md` — 참고 자료

### 포함 링크

- Bash 쉘스크립트 개발 시작하기
- 아무도 가르쳐주지 않는 리눅스 기초

### 의미

강의가 입문 실습 중심이기 때문에, 이 장은 스스로 더 확장 학습할 수 있도록 외부 자료 출발점을 제공한다.

---

## `readme.md` — 릴리스 노트 성격의 문서

이 파일은 강의 본문이 아니라 변경 이력을 적어둔 릴리스 노트다.

주요 기록:

- 2025-11-18: 01~06 강의자료 업로드
- 2025-11-19: Docker 환경 nginx 실습 추가, 07 참고 추가, QnA 정답 추가
- 2026-03-15: `vim`, 쉘 스크립트 관련 자료 추가, 전체 자료 업데이트

즉, 저장소가 한 번에 완성된 것이 아니라 점차 확장되며 현재 구조가 되었다는 점을 보여준다.

---

## `qna/` 디렉토리 정리

`qna/`에는 다음 정답 문서가 있다.

- `02_basic_answer.md`
- `03_vim_basic_answer.md`
- `04_vim_advanced_answer.md`
- `05_file_dir_answer.md`
- `06_network_script_answer.md`
- `07_script_advanced_answer.md`
- `08_permission_process_answer.md`
- `09_practical_cmd_answer.md`

특징은 아래와 같다.

### 1) 추가 실습이 있는 장의 해설 보강

강의 본문은 “직접 해보라”는 과제 제시까지 하고, `qna/`는 그 과제의 표준 풀이 흐름을 제공한다.

### 2) 답안이 과하게 복잡하지 않음

- 초보자가 따라 하기 쉽게 최소 명령 중심으로 되어 있다.
- 다만 `07_script_advanced_answer.md`처럼 일부 문서는 인자 개수 검증 등 약간 더 실전적인 요소도 포함한다.

### 3) 실습-정답 연결이 자연스러움

- 기본 명령어 → 파일 생성 정답
- `vim` 기초/심화 → 편집 절차 정답
- 파일 처리 → `grep "^ERROR"` 활용
- 네트워크/스크립트 → 실제 스크립트 파일 작성
- 권한/프로세스 → `nohup`, `screen`, `kill` 흐름 재확인
- 실무 명령어 → `du`, `df`, `cut`, `awk`, `grep`, `view`, `less`, `uptime` 종합 복습

---

## 강의 전체 흐름 요약

이 강의는 단순히 명령어를 나열하지 않고, 아래 순서로 난이도를 올린다.

### 1) 환경 구축

- Docker 기반 Ubuntu 준비
- 컨테이너 접속/재접속
- 로컬 디렉토리 마운트

### 2) CLI 기초

- `cd`, `pwd`, `ls`, `tree`
- `touch`, `cat`
- 상대경로/절대경로
- `man`, `--help`

### 3) 편집기 적응

- `vim` 기본 모드
- 저장/종료/검색
- `.vimrc`, visual mode, block edit

### 4) 파일과 로그 처리

- `cp`, `mv`, `rm`, `find`
- `head`, `tail`, `less`
- `grep`
- 파이프

### 5) 네트워크와 자동화 시작

- `ping`, `curl`, `wget`
- 환경 변수
- 실행 가능한 쉘 스크립트
- `for`, `if`

### 6) 쉘 스크립트 구조화

- 변수
- 예약 변수
- 위치 매개변수
- `case`
- `while`
- 함수
- `bash -x`

### 7) 운영 감각

- 권한과 소유자
- 프로세스 조회와 종료
- `jobs`, `kill`
- `nohup`, `screen`

### 8) 실무형 활용

- `du`, `df`
- `tar`, `zip`
- `which`, `whereis`
- `cut`, `awk`
- `history`
- `uptime`
- Docker 기반 nginx

---

## 문서 전반의 특징

### 장점

- 입문자가 따라 하기 쉬운 순차 구조
- Docker를 활용해 환경 차이를 줄임
- 실습 위주라 체감형 학습 가능
- Q&A 정답이 별도로 있어 자기 점검 가능
- 실무에서 많이 보는 도구들(`grep`, `awk`, `nohup`, `screen`, `docker -d`, `nginx`)까지 이어짐

### 강조되는 학습 습관

- 명령어를 외우기보다 직접 실행해 보기
- 결과를 `cat`, `ls`, `ps`, `tail` 등으로 반드시 확인하기
- 단발성 작업을 스크립트로 바꾸기
- 위험 명령어(`rm -rf /`, `kill -9`)는 조심스럽게 다루기

### 대상 독자

- 리눅스를 처음 접하는 입문자
- Docker 컨테이너 안에서 CLI를 익히려는 사람
- 서버/데이터/개발 환경에서 자주 쓰는 기본 명령어를 빠르게 익히고 싶은 사람

---

## 최종 정리

이 저장소의 핵심은 “실습 가능한 리눅스 입문-중급 연결형 커리큘럼”이다.

- 초반부는 환경 구축과 기본 명령어
- 중반부는 `vim`, 파일 처리, 네트워크, 쉘 스크립트
- 후반부는 권한, 프로세스, 백그라운드 작업, 실무 명령어

로 구성되어 있다.

특히 각 장이 독립적이면서도 자연스럽게 연결되어 있어, 앞 장에서 만든 파일이나 익힌 개념을 다음 장에서 다시 활용하도록 설계되어 있다. `qna/` 답안까지 함께 보면 단순 강의자료를 넘어 “혼자서 실습하고 확인하는 셀프 러닝 패키지”에 가깝다.
