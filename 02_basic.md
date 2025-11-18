# 🕐 2. 리눅스 기본 명령어 실습

## 🎯 목표

Docker로 실행된 Ubuntu 컨테이너에서  
**로컬과 공유된 디렉토리를 활용해 리눅스 기본 명령어를 익힌다.**

공유 폴더 예시:

- 로컬 경로: `~/workspace/docker/linux-lab`
- 컨테이너 내부 경로:  `cd /root/workspace   cd ~/workspace`
- `pwd`

* * *

## 📁 사전 준비 (이미 완료했다고 가정)

Docker로 Ubuntu 컨테이너 실행:

```bash
mkdir -p ~/workspace/docker/linux-lab
docker run -it \
  -v ~/workspace/docker/linux-lab:/root/workspace \
  --name linux-lab \
  ubuntu:24.04
```

컨테이너 내부 공유 디렉토리 위치:

```
/root/workspace
```

* * *

## 🧩 실습 1 — 디렉토리 이동 (cd)

```bash
cd /root/workspace
```

* * *

## 🧩 실습 2 — 현재 경로 확인 (pwd)

```bash
pwd
```

예상 결과:

```
/root/workspace
```

* * *

## 🧩 실습 3 — 파일 목록 확인 (ls)

```bash
ls
```

옵션 포함:

```bash
ls -lh
```

* * *

## 🧩 실습 4 — 디렉토리 구조 보기 (tree)

설치 필요:

```bash
apt update && apt install -y tree
```

사용:

```bash
tree
```

전체 디렉토리 구조(루트) 가볍게 둘러보기:

```bash
cd /
tree
# 출력이 너무 길면 Ctrl+C 로 중지
cd /root/workspace  # 원래 실습 경로로 복귀
```

<br>

```
# tree 결과 예시
root@64877496840f:~# tree
.
|-- big.log
|-- example.html
|-- index.html
|-- index.html.1
|-- log_writer.sh
|-- logs2
|   |-- app.log
|   `-- app2.log
|-- myapp
|   |-- app.py
|   |-- config.yaml
|   |-- readme.txt
|   `-- static
|       `-- style.css
|-- myapp.tar.gz
|-- png
|-- users.csv
`-- workspace

5 directories, 14 files
```

<br>

* * *

## 🧩 실습 5 — 파일 생성 (touch)

```bash
cd /root/workspace/
touch test.txt
ls
tree
```

로컬 `~/workspace/docker/linux-lab`에도 동일한 파일이 생성됨을 확인.

* * *

## 🧩 실습 6 — 파일 내용 출력 (cat)

```bash
echo "Hello Linux" > test.txt
cat test.txt
```

* * *

## 🧩 실습 7 — 상대경로/절대경로 이동

상위로 이동:

```bash
cd ..
pwd
```

다시 돌아오기:

```bash
cd workspace
pwd
```

바로 이전 디렉토리로 이동:

```
cd /root
pwd

cd -
pwd
```

* * *

## 🧩 실습 8 — man 과 `--help` 사용

man 사용:

```
sudo unminimize
# 중간에 안내 문구가 나오면 y 를 입력해 진행
sudo apt install -y man-db manpages
man ls
```

`--help`  사용

```
ls --help
```

둘의 차이?

- man: 공식 매뉴얼, 매우 상세
- \--help: 명령어 자체가 제공하는 요약 도움말

* * *

## 🧩 실습 9 — 컨테이너 종료

```bash
exit
```

컨테이너 재접속:

```bash
docker start -ai linux-lab
```

* * *

## 📘 실습 요약

1. `cd /root/workspace`
2. `pwd`
3. `ls`, `ls -lh`
4. `tree`
5. `touch test.txt`
6. `cat test.txt`
7. `cd ..`, `cd workspace, cd -` 
8.  `man` , `--help` 
9. 종료는 `exit`, 재접속은 `docker start -ai linux-lab`

* * *

## 🎯 추가 실습 — Hello World 파일 만들기

공유된 디렉토리 `/root/workspace` 안에서  
**"Hello World"라는 문장을 가진 `hello.txt` 파일을 만들어보세요.**

### 🔍 목표

- 파일 생성
- 내용 입력
- 파일 내용 확인

### 💡 힌트 (사용할 명령어)

- `touch`
- `echo`
- `cat`
- 리다이렉션: `>`

<br>
