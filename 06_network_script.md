# 🕓 6. 네트워크 & 쉘 스크립트 실습

## 🎯 목표

리눅스에서 **네트워크 명령어, curl 활용, wget 다운로드, 환경 변수, 쉘 스크립트, 반복문, 시스템 정보 조회**를 익힌다.

* * *

# 💻 실습

## 🧩 실습 1 — ping 설치 + 네트워크 연결 확인

Ubuntu 최소 이미지에서는 ping 패키지가 없을 수 있으므로 설치가 필요합니다.

```bash
apt update && apt install -y iputils-ping curl
```

테스트:

```bash
ping -c 4 google.com
```

* * *

## 🧩 실습 2 — curl 기본 사용

```bash
curl https://example.com
curl https://ifconfig.me   # 공인 IP 확인
```

> `ifconfig.me` 는 외부 서비스이므로 네트워크 상태나 실행 환경에 따라 응답하지 않을 수 있습니다.  
> 응답이 없으면 `curl https://ipecho.io/plain` 으로도 확인해보세요.

* * *

## 🧩 curl 테스트용 사이트 모음

| 사이트 | 용도  | 예시  |
| --- | --- | --- |
| [https://httpbin.org](https://httpbin.org) | GET/POST/헤더/리다이렉트 테스트 | `curl https://httpbin.org/get` |
| [https://postman-echo.com](https://postman-echo.com) | GET/POST 테스트 | `curl https://postman-echo.com/get?foo=bar` |
| [https://ifconfig.me](https://ifconfig.me) | 공인 IP 확인 | `curl https://ifconfig.me` |
| [https://example.com](https://example.com) | 단순 HTML 다운로드 | `curl https://example.com` |

* * *

## 🧩 curl 유용한 옵션 실습 

### ✔ `-v` : 상세 로그 출력

```bash
curl -v https://httpbin.org/get
```

### ✔ `-I` : 응답 헤더만 가져오기

```bash
curl -I https://example.com
```

### ✔ `-L` : 리다이렉트 따라가기

```bash
curl -L https://httpbin.org/redirect/2
```

### ✔ `-H` + `-d` : 헤더 추가 + POST JSON

```bash
curl -v -X POST https://httpbin.org/post   -H "Content-Type: application/json"   -d '{"name":"linux-user"}'
```

* * *

## 🧩 curl 다운로드 

example.com 의 index.html 다운로드:

```bash
curl -o index.html https://www.example.com/
```

혹은 URL 그대로 파일명으로 저장하고 싶다면:

```bash
curl -O https://www.example.com/index.html
```

다운로드 확인:

```bash
ls -l
cat index.html
```

* * *

## 🧩 실습 3 — wget 다운로드 

```bash
apt update && apt install -y wget
wget https://www.example.com/
```

index.html 확인:

```bash
ls -l
cat index.html
```

* * *

## 🧩 실습 4 — 환경 변수 확인 & 설정

```bash
cd ~
pwd

# 출력 예시
# /root

echo $PATH
export MYNAME="linux-user"
echo $MYNAME
echo 'export MYNAME="linux-user"' >> ~/.bashrc
vim .bashrc
# shift + g 로 파일 마지막으로 이동하여 export MYNAME="linux-user" 확인
```

* * *

### 💡 `.bash_profile` vs `.bashrc` 차이점

- **`.bash_profile` (로그인 쉘)**: 시스템에 로그인할 때 한 번만 실행됩니다. 주로 전체 환경 변수(`PATH` 등)를 설정할 때 사용합니다.
- **`.bashrc` (비로그인 쉘)**: 터미널 창(또는 bash 세션)을 새로 열 때마다 매번 실행됩니다. 주로 단축 명령어(`alias`)나 터미널 동작 설정을 추가합니다.
- (참고) Ubuntu 등 여러 시스템에서는 로그인 시 `.bash_profile` 파일 내부에서 `.bashrc`를 호출하도록 구성되어 있습니다.

* * *
## 🧩 실습 5 — Hello World 스크립트

```bash
cd /root/workspace
cat << 'EOF' > hello.sh
#!/bin/bash
echo "Hello World from Shell Script"
EOF

chmod +x hello.sh
./hello.sh
```

- `chmod +x hello.sh` : 실행 권한을 추가합니다.
- 실행 권한이 없으면 `./hello.sh` 처럼 직접 실행할 수 없습니다.

* * *

## 🧩 실습 6 — 날짜 기반 로그 자동 생성 스크립트

```bash
cat << 'EOF' > create_daily_log.sh
#!/bin/bash
TODAY=$(date +"%Y%m%d")
echo "Log created at $(date)" > log_$TODAY.txt
EOF

chmod +x create_daily_log.sh
./create_daily_log.sh
ls

# cat log_YYYYMMDD.txt 확인
```

- `chmod +x create_daily_log.sh` : 스크립트를 프로그램처럼 실행할 수 있게 만듭니다.

* * *

## 🧩 실습 7 — 반복문 for

```bash
for i in {1..3}; do 
  echo "File $i" > file$i.txt
done
ls #files 확인
```

* * *

## 🧩 실습 8 — 조건문 if

```bash
cat << 'EOF' > check_disk.sh
#!/bin/bash
USE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ -z "$USE" ]; then
  echo "디스크 사용량을 확인하지 못했습니다."
elif [ "$USE" -gt 70 ]; then
  echo "Warning: Disk usage high! ($USE%)"
else
  echo "Disk usage normal ($USE%)"
fi
EOF

chmod +x check_disk.sh
./check_disk.sh
```

- `sed 's/%//'` : 사용량 문자열에서 `%` 문자를 제거해 숫자만 남깁니다.
- `chmod +x check_disk.sh` : 스크립트에 실행 권한을 부여합니다.

* * *

## 🧩 실습 9 — 시스템 정보 조회

```bash
top
free -h
df -h
ps aux
```

* * *

## ⚠️ 주의 사항

- 스크립트 실행 시 `./script.sh` 사용
- 환경 변수는 export만 하면 세션 종료 시 사라짐 → `.bashrc`에 추가

* * *

## 📘 실습 요약

| 기능 | 명령어 / 문법 |
|------|-------------|
| 네트워크 연결 확인 | `ping` |
| HTTP 요청 | `curl`, `curl -v/I/L/X` |
| 웹에서 파일 다운로드 | `wget`, `curl -o`, `curl -O` |
| 환경 변수 등록/확인 | `export`, `echo $PATH` |
| 쉘 스크립트 작성/실행 | `#!/bin/bash`, `chmod +x`, `./script.sh` |
| 반복문 (for) | `for i in {1..3}; do ... done` |
| 조건문 (if) | `if [ 조건 ]; then ... else ... fi` |
| 시스템 리소스 모니터링 | `top`, `free`, `df`, `ps` |

* * *

## 🎯 추가 실습

```
1) ifconfig.me를 활용해 자신의 공인 IP를 확인하고,
해당 결과를 my_ip.txt 파일로 저장하는 쉘 스크립트(get_ip.sh)를 작성해보세요.
```

### 💡 힌트
- 공인 IP 확인: `curl https://ifconfig.me`
- 결과를 파일로 바로 넘기기(저장): `> my_ip.txt`
- 쉘 스크립트 실행 권한 부여: `chmod +x`
- 외부 서비스가 응답하지 않으면 `curl https://ipecho.io/plain` 으로 대체 가능
