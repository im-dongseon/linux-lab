# ✅ 정답 (05_file_dir.md)

## Q. log 폴더를 만들고 system.log 파일에서 ERROR로 시작하는 줄만 모아 log/error_summary.txt 파일로 저장하세요.

## A.

```bash
# 1. log 폴더 생성
mkdir log

# 2. error로 시작하는 줄만 모아 저장
grep "^ERROR" system.log > log/error_summary.txt

# 3. 결과 확인
cat log/error_summary.txt
```

* * *

## Q. a.txt 는 hello 문장을, b.txt 는 world 문장을 가진 파일을 만들고 병합해보세요. 

## A.

```bash
echo "hello" > a.txt
echo "world" > b.txt
# 병합
cat a.txt b.txt > all.txt
```
