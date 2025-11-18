# 🕑 5. 파일·디렉토리 다루기 실습

## 🎯 목표

리눅스에서 **파일 생성, 이동, 삭제, 복사, 검색, 출력, 텍스트 처리, 파이프 사용**을 익히고  
head/tail 실습을 위해 실제 로그 파일을 직접 생성해본다.

* * *

# 💻 실습

* * *

## 📁 사전 준비 — 로그 파일 생성하기 (head/tail 준비)

100줄짜리 로그 파일 생성:

```bash
for i in {1..100}; do 
  echo "INFO Line $i - Sample log message"; 
done > sample.log
```

실제 같은 로그 파일:

```bash
for i in {1..150}; do
  if (( i % 15 == 0 )); then
    echo "ERROR Something failed at step $i"
  elif (( i % 10 == 0 )); then
    echo "WARN Potential issue detected at $i"
  else
    echo "INFO Normal operation line $i"
  fi
done > system.log
```

* * *

## 🧩 실습 1 — 파일 복사 (cp)

```bash
cp system.log backup.txt

mkdir src_folder
cp ./backup.txt src_folder/
ls src_folder

mkdir src_folder/test
ls src_folder
cp src_folder test_folder # -r 옵션이 없으면 디렉토리는 복사되지 않음
cp -r src_folder dst_folder
ls dst_folder
tree
```

* * *

## 🧩 실습 2 — 파일 이동 또는 이름 변경 (mv)

```bash
mv backup.txt newname.txt
ls
mkdir -p /root/workspace/logs/
mv newname.txt /root/workspace/logs/
ls /root/workspace/logs/
```

> `logs` 디렉토리가 없는데 먼저 `mv newname.txt /root/workspace/logs/` 를 실행하면 오류가 발생합니다.  
> 디렉토리를 만든 뒤 다시 실행하면 정상적으로 이동됩니다.

<br>

* * *

## 🧩 실습 3 — 파일 삭제 (rm)

```bash
cp system.log file.txt
rm file.txt
rm dst_folder # -r 차이점 확인
rm -r dst_folder
rm -rf src_folder
```

### ⚠️ 삭제 명령의 원리 설명

- `rm`은 휴지통이 없음
- 삭제 즉시 파일이 사라짐
- `rm -r`은 폴더까지 재귀적으로 삭제

### ❌ 절대 입력하면 안 되는 명령어

```bash
rm -rf /
```

**이 명령은 리눅스 시스템 전체를 삭제하는 최악의 명령어로,  
Docker 컨테이너든 실제 서버든 시스템이 완전히 파괴됩니다.**

- `/` = 시스템 전체 루트 디렉토리
- `rm -rf` = 묻지도 따지지도 않고 강제 삭제

* * *

## 🧩 실습 4 — 파일 검색 (find)

```bash
cp system.log file.txt
find . -name "*.txt"
find / -name "*.txt"
# 파일 이름이 error로 시작하는 파일 검색
find /root/workspace -type f -name "error*"
```

* * *

## 🧩 실습 5 — 파일 내용 출력 (cat)

```bash
cat system.log
```

* * *

## 🧩 실습 6 — 파일 일부 보기 (head / tail)

```bash
head system.log
tail system.log
```

대용량 파일(옵션):

```bash
for i in {1..1000}; do echo "DEBUG $i"; done > big.log
head big.log
tail big.log
```

* * *

## 🧩 실습 7 — 파일을 페이지 단위로 보기 (less)

```bash
apt update && apt install -y less
less system.log
```

(종료: `q`)

* * *

## 🧩 실습 8 — 여러 파일 합치기

```bash
echo "hello" > a.txt
echo "world" > b.txt
cat a.txt b.txt > all.txt
cat all.txt
```

* * *

## 🧩 실습 9 — 특정 단어 포함된 줄 찾기 (grep)

```bash
grep "error" system.log  # 안나옴
grep "ERROR" system.log
grep -i "error" system.log
```

특정 디렉토리 내의 모든 파일에서 내용 검색하기:
```bash
grep -ril "error" /root/workspace
```
- `-r` : 하위 디렉토리까지 재귀적으로 검색
- `-i` : 대소문자 무시
- `-l` : 매칭되는 내용이 있는 파일의 이름만 출력


* * *

## 🧩 실습 10 — 파이프 사용 (|)

파이프(`|`)는 앞 명령어의 출력 결과를 뒤 명령어의 입력으로 전달합니다.
중간에 임시 파일을 만들 필요 없이 여러 명령어를 자연스럽게 연결할 때 필수적으로 사용됩니다.

```bash
cat system.log | grep -i "error"
grep "WARN" system.log | head
grep "WARN" system.log 
```

* * *

## 📘 실습 요약

| 기능  | 명령어 |
| --- | --- |
| 복사  | `cp`, `cp -r` |
| 이동/이름 변경 | `mv` |
| 삭제  | `rm`, `rm -r` |
| 검색  | `find` |
| 출력  | `cat`, `head`, `tail`, `less` |
| 텍스트 필터링 | `grep` |
| 파일 병합 | `cat a b > c` |
| 파이프 | `\|`  |
| 로그 생성 | `echo`, `for`, 리다이렉션 |

* * *

## 🎯 추가 실습

```
1)log 폴더를 만들고  
system.log 파일에서  
ERROR로 시작하는 줄만 모아  
log/error_summary.txt 파일로 저장하세요.

2) a.txt 는 hello 문장을, 
b.txt 는 world 문장을 가진 파일을 만들고 병합해보세요. 
```

### 힌트

- `mkdir log`
- `grep "^ERROR"`
- `cat` 
- `>` 리다이렉션
