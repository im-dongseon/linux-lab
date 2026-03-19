# ✅ 정답 (08_script_advanced.md)

## Q. 함수, 사용자 정의 변수, 위치 매개변수를 종합하여 시스템 사용자 추가 시뮬레이션 스크립트(create_users.sh)를 작성하세요.

## A.

```bash
cat << 'EOF' > create_users.sh
#!/bin/bash

# 사용자 정의 함수 선언
add_user() {
  echo "사용자 $1를 시스템에 추가합니다."
}

# 인자가 2개인지 확인 (선택적 예외 처리)
if [ $# -ne 2 ]; then
  echo "사용법: $0 <아이디1> <아이디2>"
  exit 1
fi

# 위치 매개변수로 넘어온 두 인자를 이용해 함수를 각각 호출
add_user $1
add_user $2
EOF

chmod +x create_users.sh
./create_users.sh alice bob
```
