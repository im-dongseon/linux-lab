# 🕓 9. 실무에서 자주 쓰는 명령어 실습

## 🎯 목표

서버 운영·배포·장애 대응에서 **실제로 자주 사용하는 리눅스 명령어**들을 실습 위주로 익힌다.

* * *

## 🧩 실습 1 — 디렉토리 용량 확인 (du)

```bash
du -sh .
du -sh *
du -sh logs/*
```

> 디스크 Full 이슈가 생겼을 때, “어느 폴더가 많이 먹는지” 확인하는 데 활용.

* * *

## 🧩 실습 2 — 전체 디스크 용량 확인 (df)

```bash
df -h
df -h /
```

- 전체 디스크 파티션 사용량 확인
- `-h`: GB/MB 단위로 보기

* * *

## 🧩 실습 3 — tar / zip 실습용 테스트 디렉토리 생성

```bash
mkdir myapp
touch myapp/app.py
touch myapp/config.yaml
mkdir myapp/static
touch myapp/static/style.css
echo "hello linux" > myapp/readme.txt
ls -R myapp
```

* * *

## 🧩 실습 4 — tar 압축 / 해제

```bash
tar -czvf myapp.tar.gz myapp/
tar -xzvf myapp.tar.gz
```

- `c` : 생성(create)
- `z` : gzip 압축
- `v` : 진행 상황 출력
- `f` : 파일 이름 지정

* * *

## 🧩 실습 5 — zip / unzip

```bash
apt update && apt install -y zip unzip

zip -r myapp.zip myapp/
unzip myapp.zip
```

* * *

## 🧩 실습 6 — python 설치 (which 실습 준비)

```bash
apt update && apt install -y python3
python3 --version
```

* * *

## 🧩 실습 7 — which (명령어 위치 확인)

```bash
which python3
which curl
which tar
which nginx
```

출력 예시:

```
/usr/bin/python3
/usr/bin/curl
```

> PATH 상에서 **실제로 어떤 실행 파일이 쓰이는지** 확인할 때 사용.

* * *

## 🧩 실습 8 — whereis (명령/문서/라이브러리 위치 검색)

```bash
whereis python3
whereis nginx
whereis bash
```

예시:

```
python3: /usr/bin/python3 /usr/lib/python3.10 /usr/share/man/man1/python3.1.gz
```

> `which` 는 실행 파일 한 개 경로 중심,  
> `whereis` 는 실행 파일 + man 페이지 + 관련 파일까지 같이 보여줌.

* * *

## 🧩 실습 9 — less vs tail (로그 보기 방식 비교)

테스트 로그 만들기:

```bash
for i in {1..50}; do echo "LINE $i - sample log" >> big.log; done
```

### tail

```bash
tail big.log
tail -n 20 big.log
```

- **파일의 끝부분** 위주로 짧게 확인할 때 사용

### less

```bash
apt update && apt install -y less

less big.log
```

- `↑ / ↓` : 한 줄씩 이동
- `Space` : 다음 페이지
- `g` : 맨 위로
- `G` : 맨 아래로
- `/문자열` : 검색
- `q` : 종료

> tail 은 “마지막 몇 줄만 잠깐 보는 용도”,  
> less 는 “긴 로그를 위아래로 오가며 보는 용도”.

* * *

## 🧩 실습 10 — grep 심화 옵션 (-r, -n, -i)

테스트용 디렉토리/파일 생성:

```bash
mkdir logs2
echo -e "Error: something failed\nINFO: all good\nerror: lower case" > logs2/app.log
echo -e "WARN: be careful\nINFO: done" > logs2/app2.log
```

- `echo -e` 는 `\n` 같은 이스케이프 문자를 실제 줄바꿈으로 해석할 때 사용합니다.

### 1) -n : 줄 번호 함께 출력

```bash
grep -n "Error" logs2/app.log
```

### 2) -i : 대소문자 무시

```bash
grep -i "error" logs2/app.log
```

### 3) -r : 하위 디렉토리까지 재귀적으로 검색

```bash
grep -rn "INFO" logs2
```

> \-r, -n, -i 조합은 **로그 검색의 필수 기본기**.

* * *

## 🧩 실습 11 — cut + CSV 파일로 컬럼 추출

먼저 CSV 파일을 하나 만든다:

```bash
cat << 'EOF' > users.csv
id,name,age,city
1,Alice,30,Seoul
2,Bob,25,Busan
3,Charlie,35,Incheon
EOF
```

### 쉼표로 구분된 1,3번째 컬럼만 보기

```bash
cut -d',' -f1,3 users.csv
```

- `-d','` : 구분자를 `,` 로 지정
- `-f1,3` : 1번, 3번 컬럼 선택

> CSV, 로그, 간단한 텍스트 데이터 전처리에 매우 자주 사용.

* * *

## 🧩 실습 12 — awk 기초 (컬럼 기반 처리)

users.csv 를 다시 활용:

```bash
awk -F',' '{print $1, $2}' users.csv
```

- `-F','` : 구분자 comma
- `$1`, `$2` : 첫 번째, 두 번째 컬럼

헤더를 제외하고 나이(age) 30 이상인 유저만 출력:

```bash
awk -F',' 'NR>1 && $3 >= 30 {print $2, $3}' users.csv
```

- `NR>1` : 첫 줄(헤더) 제외
- `$3 >= 30` : 3번째 컬럼(age)이 30 이상인 행만 선택

> grep 이 “문자열 검색”이라면, awk는 “컬럼 단위 데이터 처리”에 강함.

* * *

## 🧩 실습 13 — view (읽기 전용 vim)

`view` 는 사실상 `vim -R` (read-only 모드)와 같다.

```bash
view users.csv
```

특징:

- 파일 내용을 **읽기 전용으로 열기**
- 실수로 수정하는 것을 방지하고 싶을 때 사용
- vim 안에서도 `:view` 나 `:set readonly` 등을 사용할 수 있음

> 로그 파일이나 설정 파일을 “보기만” 할 때 유용.

* * *

## 🧩 실습 14 — history / !! / !n / history | grep

전체 히스토리:

```bash
history
```

특정 명령어만 보기:

```bash
history | grep tar
history | grep python
```

직전 명령 재실행:

```bash
!!
```

히스토리 번호로 실행:

```bash
!25  # (번호는 개인 터미널 세션마다 다름)
```

> 자주 쓰는 긴 명령을 반복할 때 생산성이 크게 올라감.

* * *

## 🧩 실습 15 — uptime (서버 가동 시간 / 부하 확인)

```bash
uptime
```

예시 출력:

```
 10:23:45 up 5 days,  3:12,  2 users,  load average: 0.12, 0.08, 0.05
```

- up 5 days : 5일 동안 꺼지지 않고 동작 중
- load average : 최근 1분 / 5분 / 15분 평균 부하

> 운영 중인 서버의 상태를 빠르게 파악할 수 있는 간단하지만 유용한 명령어.

* * *

## 🧩 실습 16 — 서비스 관리 (service + nginx)

Docker Ubuntu 환경에서는 `systemctl` 이 동작하지 않으므로  
`service` 로 nginx 를 테스트한다.

설치:

호스트(Mac/Windows 등 사용자의 로컬 터미널)에서 먼저 컨테이너를 실행합니다.

```bash
docker run -it -p 8080:80 --name test-nginx ubuntu:24.04
```

이후 `test-nginx` 컨테이너 내부에서 nginx를 설치합니다.

```bash
apt update && apt install -y nginx
```

상태 확인:

```bash
service nginx status
```

시작 / 중지 / 재시작:

```bash
service nginx start
service nginx stop
service nginx restart
```

* * *

## 🧩 실습 17 — Docker 환경에서 nginx 띄우고 접속하기

> **⚠️ 주의**: 아래의 `docker run` 명령어들은 우분투 컨테이너 내부가 아닌 **호스트(Mac/Windows 등 사용자의 로컬 터미널)에서 실행**해야 합니다. (컨테이너 안에서는 docker 명령어가 동작하지 않습니다.)

### 1) nginx가 설치된 Ubuntu 컨테이너를 직접 띄우는 경우

#### ✔ Ubuntu 컨테이너 실행 (포트 매핑 포함)

```bash
docker run -it -p 8080:80 --name ubuntu-nginx ubuntu:24.04
```

**옵션 설명**

- `-i` : interactive, 표준 입력 유지
- `-t` : TTY 할당 → bash 접근 가능
- `-p 8080:80` :
    - 호스트 8080 → 컨테이너 80
    - 브라우저는 호스트의 `8080` 포트로 접속하고, 실제 nginx 는 컨테이너 내부 `80` 포트에서 요청을 받습니다.
    - 브라우저에서 [http://localhost:8080](http://localhost:8080) 접속 가능
- `--name ubuntu-nginx` : 컨테이너 이름 지정
- `ubuntu:24.04` : 사용할 이미지

* * *

#### ✔ 컨테이너 내부에서 nginx 설치 및 실행

```bash
apt update && apt install -y nginx
nginx
```

#### ✔ 프로세스 확인

```bash
ps aux | grep nginx
```

#### ✔ 호스트에서 접속 테스트

```bash
curl localhost:8080
```

브라우저:

```
http://localhost:8080
```

* * *

### 2) nginx 공식 이미지 사용 (추천 패턴)

실무에서는 Ubuntu 이미지를 쓰기보다는, **nginx 공식 이미지**를 바로 실행하는 경우가 훨씬 많다.

* * *

#### ✔ nginx 컨테이너 실행

```bash
docker run -d -p 8080:80 --name my-nginx nginx
```

**옵션 설명**

- `-d` : detached mode
    - **컨테이너를 백그라운드에서 실행**
    - nginx 같은 웹 서버는 계속 실행되어야 하므로 필수적
    - 터미널이 즉시 반환되어 다른 명령을 계속 입력할 수 있음
- `-p 8080:80` : 호스트 8080 → 컨테이너 80
- `--name my-nginx` : 컨테이너 이름
- `nginx` : Docker Hub 공식 nginx 이미지

* * *

#### ✔ 접속 테스트

```bash
curl localhost:8080
```

브라우저에서도:

```
http://localhost:8080
```

* * *

#### ✔ 로그 확인

```bash
docker logs -f my-nginx
```

- `-f`: tail -f 처럼 실시간 로그 스트림 보기

* * *

#### ✔ 컨테이너 안으로 들어가기

```bash
docker exec -it my-nginx bash
```

* * *

#### ✔ 종료 및 삭제

```bash
docker stop my-nginx
docker rm my-nginx
```

* * *

# 📌 -d 옵션을 사용하는 이유

| 실행 방식 | 명령 예 | 설명  |
| --- | --- | --- |
| 포그라운드 | `docker run -it ubuntu` | 컨테이너와 터미널이 붙어서 별도 작업 어려움 |
| 백그라운드(-d) | `docker run -d nginx` | nginx가 **백그라운드 데몬처럼 계속 실행**, 터미널은 계속 사용 가능 |

**→ nginx, API 서버, DB 등 “지속 실행형 서비스”에는 -d 옵션이 거의 필수**

* * *

## 📘 실습 요약

| 기능 | 명령어 |
|------|------|
| 디스크/용량 확인 | `du -sh`, `df -h` |
| 파일 압축/해제 | `tar -czvf <archive.tar.gz> <dir>`, `tar -xzvf <archive.tar.gz>`, `zip -r`, `unzip` |
| 명령어 위치 추적 | `which`, `whereis` |
| 로그/텍스트 조회 | `less` (페이지이동), `view` (읽기전용) |
| 필터/텍스트 처리 | `grep -rin`, `cut -d',' -f1,3`, `awk -F','` |
| 명령어 히스토리 | `history`, `!!`, `!번호` |
| 시스템 상태 확인 | `uptime` |
| 컨테이너 모드 | `docker run -d` (백그라운드 데몬) |

* * *

## 🎯 추가 실습 (종합 미션)

1. `du` + `df`를 이용해 현재 디스크 상태를 점검해보고, 용량이 가장 큰 디렉토리 3개를 찾아보세요.
2. `myapp` 폴더를 tar.gz와 zip 두 가지 포맷으로 압축하고, 다시 풀어보세요.
3. `users.csv`에서 `cut`과 `awk`를 이용해 `(이름, 나이)` 목록만 뽑아보세요.
4. `logs2` 디렉토리에서 `grep -rn -i "error"` 로 에러 로그 위치를 찾아보세요.
5. `view`를 사용해서 users.csv를 읽기 전용으로 열고, `less`로 big.log를 살펴본 뒤, `uptime`으로 서버 상태를 확인해보세요.

<br>
