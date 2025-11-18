# ✅ 정답 (03_vim_basic.md)

## Q. note.txt 파일에 3줄을 추가하고, 2번째 줄을 dd로 삭제한 뒤 저장해보세요.

## A.

```bash
vim note.txt

# 1. G 로 파일 끝으로 이동
# 2. o 로 새 줄 INSERT → 3줄 입력 → Esc
# 3. 파일 맨 위로 이동: gg
# 4. 2번째 줄로 이동: 2G (또는 j 한 번)
# 5. dd 로 해당 줄 삭제
# 6. :wq 저장 후 종료

cat note.txt
```
