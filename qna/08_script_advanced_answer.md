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

# 인자가 하나도 없는 경우 예외 처리
if [ $# -eq 0 ]; then
  echo "사용법: $0 <아이디1> [아이디2 ...]"
  exit 1
fi

# 위치 매개변수로 넘어온 두 인자를 이용해 함수를 각각 호출
#add_user $1
#add_user $2

# while 문을 사용해 넘어온 모든 인자를 순회하며 함수 호출
while [ $# -gt 0 ]; do
  add_user "$1"
  shift
done
EOF

chmod +x create_users.sh
./create_users.sh alice bob
```
