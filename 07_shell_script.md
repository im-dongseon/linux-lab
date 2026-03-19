# 🕓 7. 기초 쉘 스크립트 실습

## 🎯 목표

리눅스에서 **터미널과 쉘의 차이**를 이해하고, **환경 변수, 기초 쉘 스크립트 작성, 반복문, 조건문, 시스템 정보 조회**를 익힌다.

---

## 📖 개념 학습: 터미널 에뮬레이터 vs 쉘 (Shell)

흔히 '터미널을 연다'고 말할 때, 실제로는 **터미널 에뮬레이터(Terminal Emulator)**와 **쉘(Shell)**이라는 두 가지 프로그램이 함께 작동합니다.

1. **터미널 (Terminal Emulator)**
   - 사용자가 키보드로 입력한 글자를 받아들이고, 결과를 화면(창)에 텍스트로 그려주는 **껍데기 UI 프로그램**입니다.
   - 예: macOS의 `Terminal.app`, `iTerm2`, Windows의 `Windows Terminal`, 우분투의 `GNOME Terminal` 등.
   - 단축키, 창 분할, 폰트 색상, 테마 등은 터미널 프로그램이 관리합니다.

2. **쉘 (Shell)**
   - 터미널을 통해 입력받은 **명령어를 해석하여 운영체제(커널)에 전달**하고, 그 결과를 터미널로 돌려보내는 실제 **명령어 처리기**입니다.
   - 쉘 스크립트를 작성한다는 것은 이 쉘이 이해할 수 있는 언어로 명령을 연속해서 적어두는 것을 의미합니다.

---

## 📖 개념 학습: Bash vs Zsh

리눅스 및 유닉스 환경에는 여러 종류의 쉘이 존재하며, 보통 그중 하나를 기본 쉘로 사용합니다.

- **Bash (Bourne Again SHell)**
  - 오랫동안 거의 모든 리눅스 배포판(Ubuntu, CentOS 등)의 기본 쉘로 자리매김한 가장 표준적인 쉘입니다.
  - 호환성이 가장 높기 때문에 쉘 스크립트를 작성할 때는 보통 `#!/bin/bash`를 선언하여 bash용으로 작성하는 것이 안전합니다.
- **Zsh (Z Shell)**
  - Bash의 기능을 포함하면서도 자동 완성, 테마 플러그인(Oh My Zsh 등), 색상 강조 등 편의성이 크게 향상된 쉘입니다.
  - 근래 최신 macOS의 기본 쉘이 bash에서 zsh로 변경되었습니다.
- **실무 팁**: 개인 작업 환경(내 로컬 PC)에서는 편의성이 좋은 Zsh를 쓰고, 실제 서버에서 스크립트를 돌려야 할 때는 표준인 Bash 기준으로 스크립트를 작성하는 분리된 패턴을 많이 사용합니다.

---

# 💻 실습

## 🧩 실습 1 — 환경 변수 확인 & 설정

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

---

### 💡 `.bash_profile` vs `.bashrc` 차이점

- **`.bash_profile` (로그인 쉘)**: 시스템에 로그인할 때 한 번만 실행됩니다. 주로 전체 환경 변수(`PATH` 등)를 설정할 때 사용합니다.
- **`.bashrc` (비로그인 쉘)**: 터미널 창(또는 bash 세션)을 새로 열 때마다 매번 실행됩니다. 주로 단축 명령어(`alias`)나 터미널 동작 설정을 추가합니다.
- (참고) Ubuntu 등 여러 시스템에서는 로그인 시 `.bash_profile` 파일 내부에서 `.bashrc`를 호출하도록 구성되어 있습니다.

---

## 🧩 실습 2 — Hello World 스크립트

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

---

## 🧩 실습 3 — 날짜 기반 로그 자동 생성 스크립트

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

---

## 🧩 실습 4 — 반복문 for

```bash
for i in {1..3}; do 
  echo "File $i" > file$i.txt
done
ls #files 확인
```

---

## 🧩 실습 5 — 조건문 if

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

---

## 🧩 실습 6 — 시스템 정보 조회

```bash
top
free -h
df -h
ps aux
```

---

## ⚠️ 주의 사항

- 스크립트 실행 시 `./script.sh` 사용
- 환경 변수는 export만 하면 세션 종료 시 사라짐 → `.bashrc`에 추가

---

## 📘 실습 요약

| 기능 | 명령어 / 문법 |
|------|-------------|
| 환경 변수 등록/확인 | `export`, `echo $PATH` |
| 쉘 스크립트 작성/실행 | `#!/bin/bash`, `chmod +x`, `./script.sh` |
| 반복문 (for) | `for i in {1..3}; do ... done` |
| 조건문 (if) | `if [ 조건 ]; then ... else ... fi` |
| 시스템 리소스 모니터링 | `top`, `free`, `df`, `ps` |

---

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
