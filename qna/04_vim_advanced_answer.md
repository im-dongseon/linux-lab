# ✅ 정답 (04_vim_advanced.md)

## Q. .vimrc를 설정하고 vim으로 shell script 파일을 만들어 직접 실행해보세요.

## A.

```bash
# 1. .vimrc 작성
vim ~/.vimrc
# (set number, set tabstop=4, set expandtab, syntax on 등 입력 후 :wq)

# 2. 스크립트 작성
vim hello.sh
# #!/bin/bash
# echo "Hello from vim!"
# :wq

# 3. vim 안에서 실행 (저장 후)
# :!bash %
```

* * *

## Q. visual block 으로 3줄의 앞에 # 을 추가해 주석 처리 해보세요.

## A.

```text
1. Ctrl+v 로 visual block 모드 진입
2. j 두 번 눌러 3줄 선택
3. I (대문자 i) 로 줄 앞 INSERT 모드
4. # 입력
5. Esc → 3줄 모두에 # 추가됨 확인
```
