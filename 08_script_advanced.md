# 🕓 8. 쉘 스크립트 심화

## 🎯 목표

쉘 스크립트 작성에 필요한 심화 문법(함수, 변수, 반복문, 제어문, 디버깅 등)을 익히고, 보다 복잡한 자동화 스크립트를 작성할 수 있는 능력을 기른다.

* * *

## 🧩 실습 1 — 변수: 필요성, 작성 방법, 사용법

쉘 스크립트에서 데이터를 임시로 저장하고 재사용하기 위해 변수를 사용합니다.

```bash
cat << 'EOF' > test_var.sh
#!/bin/bash
# 변수 선언 (공백 없이 작성)
GREETING="Hello, Shell Script!"
COUNT=5

# 변수 사용 ($를 붙임)
echo $GREETING
echo "We have $COUNT apples."
EOF

chmod +x test_var.sh
./test_var.sh
```

* * *

## 🧩 실습 2 — 변수 심화 (환경 변수, 예약 변수)

쉘에는 미리 정의된 특별한 환경 변수나 예약 변수들이 존재합니다.

```bash
cat << 'EOF' > test_env.sh
#!/bin/bash
echo "현재 사용자: $USER"
echo "현재 디렉토리: $PWD"
echo "홈 디렉토리: $HOME"
echo "랜덤 숫자: $RANDOM"
EOF

chmod +x test_env.sh
./test_env.sh
```

* * *

## 🧩 실습 3 — 위치 매개변수 (Arguments)

스크립트를 실행할 때 파라미터(인자, Argument)를 전달하여 스크립트 내부에서 사용할 수 있습니다.
예를 들어 `./calc.sh 15 30` 처럼 실행하면 `15` 가 `$1`, `30` 이 `$2` 로 전달됩니다.

```bash
cat << 'EOF' > calc.sh
#!/bin/bash
echo "실행한 스크립트 이름: $0"
echo "첫 번째 인자: $1"
echo "두 번째 인자: $2"
echo "전달된 전체 인자 개수: $#"

# 덧셈 결과 출력
SUM=$(($1 + $2))
echo "$1 + $2 = $SUM"
EOF

chmod +x calc.sh
./calc.sh 15 30
```

* * *

## 🧩 실습 4 — 조건/선택문 (case 문)

여러 조건 중 하나를 선택해야 할 때 `if-elif` 대신 `case` 문을 사용하면 코드가 깔끔해집니다.

```bash
cat << 'EOF' > select_menu.sh
#!/bin/bash
echo "원하는 동작을 입력하세요 (start/stop/restart) : "
read ACTION

case $ACTION in
  start)
    echo "서비스를 시작합니다."
    ;;
  stop)
    echo "서비스를 중지합니다."
    ;;
  restart)
    echo "서비스를 재시작합니다."
    ;;
  *)
    echo "잘못된 입력입니다: $ACTION"
    ;;
esac
EOF

chmod +x select_menu.sh
# ./select_menu.sh 실행 후 start 등을 직접 입력해보세요.
```

* * *

## 🧩 실습 5 — 반복문 (while 루프)

조건이 참(True)인 동안 계속해서 명령을 반복하는 `while` 루프입니다.

```bash
cat << 'EOF' > while_test.sh
#!/bin/bash
NUM=1

while [ $NUM -le 3 ]
do
  echo "현재 숫자: $NUM"
  NUM=$((NUM + 1))
done
EOF

chmod +x while_test.sh
./while_test.sh
```

* * *

## 🧩 실습 6 — 함수: 필요성, 작성 방법, 사용법

쉘 스크립트가 길어질 때, 반복되는 코드를 묶어 블록으로 만들고 필요할 때 재사용할 수 있도록 함수를 사용합니다.

```bash
cat << 'EOF' > functions.sh
#!/bin/bash

# 함수 정의
print_info() {
  echo "[INFO] $1"
}

# 함수 호출 (소괄호 없이 띄어쓰기로 인자 전달)
print_info "시스템 점검을 시작합니다."
print_info "데이터베이스를 백업 중입니다..."
EOF

chmod +x functions.sh
./functions.sh
```

* * *

## 🧩 실습 7 — 쉘 스크립트 디버깅 (bash -x)

작성한 스크립트에 오류가 있거나 동작 과정을 한 줄씩 추적하고 싶을 때 `-x` 옵션을 사용합니다.

```bash
bash -x while_test.sh
```
> `+` 기호와 함께 각 줄이 쉘에서 어떻게 전개되고 실행되는지 출력됩니다.

* * *

## 📘 실습 요약

| 기능 | 문법 / 명령어 |
|------|-------------|
| 변수 선언/사용 | `NAME="value"`, `echo $NAME` |
| 시스템 예약 변수 | `$USER`, `$PWD`, `$HOME` |
| 위치 매개변수 | `$0`(스크립트명), `$1, $2...`(인자), `$#`(인자 수) |
| 선택문 | `case $변수 in ... esac` |
| 반복문 (while) | `while [ 조건 ]; do ... done` |
| 함수 작성/호출 | `func_name() { ... }`, `func_name arg1` |
| 디버깅 모드 | `bash -x script.sh` |

* * *

## 🎯 추가 실습 (종합 미션)

배운 내용 중 **함수, 사용자 정의 변수, 위치 매개변수**를 종합하여 다음 쉘 스크립트(`create_users.sh`)를 작성해보세요.
```
- 스크립트로 2개의 아이디(문자열)를 넘깁니다. (예: ./create_users.sh alice bob)
- 스크립트 내부에 'add_user' 라는 이름의 함수를 하나 만듭니다.
- 'add_user' 함수는 인자를 1개 전달받아 "사용자 [아이디]를 시스템에 추가합니다." 라고 출력해야 합니다.
- 스크립트 메인 부분에서 위치 매개변수($1, $2)를 읽고, 전달받은 두 아이디를 이용해 'add_user' 함수를 연속으로 호출하세요.
```

### 💡 힌트
- 스크립트로 넘어온 2개의 인자는 내부에서 순서대로 `$1`, `$2` 로 접근할 수 있습니다.
- 'add_user' 라는 함수를 먼저 선언(`add_user() { ... }`)하고, 내부에서도 전달받은 인자를 `$1`로 처리합니다.
- 마지막으로 스크립트 본문에서 함수 호출 시 `add_user $1`, `add_user $2` 의 형태로 차례대로 함수를 실행하도록 구현합니다.
