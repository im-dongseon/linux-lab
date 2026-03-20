# ✅ 정답 (10_practical_cmd.md)

## Q. `du` + `df`를 이용해 현재 디스크 상태를 점검해보고, 용량이 가장 큰 디렉토리 3개를 찾아보세요.

## A.

```bash
df -h
du -sh * | sort -h | tail -3
```

* * *

## Q. `myapp` 폴더를 tar.gz와 zip 두 가지 포맷으로 압축하고, 다시 풀어보세요.

## A.

```bash
tar -czvf myapp.tar.gz myapp/
tar -xzvf myapp.tar.gz

zip -r myapp.zip myapp/
unzip myapp.zip
```

* * *

## Q. `users.csv`에서 `cut`과 `awk`를 이용해 `(이름, 나이)` 목록만 뽑아보세요.

## A. 

```bash
cut -d',' -f2,3 users.csv

awk -F',' 'NR>1 {print $2, $3}' users.csv
```

* * *

## Q. `logs2` 디렉토리에서 `grep -rn -i "error"` 로 에러 로그 위치를 찾아보세요.

## A.

```bash
grep -rni "error" logs2
```

* * *

## Q. `sed -i`를 사용해 `users.csv`의 특정 도시 이름(예: Seoul)을 다른 도시(예: Jeju)로 치환해 보세요.

## A.

```bash
sed -i 's/Seoul/Jeju/g' users.csv
# 치환된 내용 확인
cat users.csv
```

* * *

## Q. `view`를 사용해서 users.csv를 읽기 전용으로 열고, `less`로 big.log를 살펴본 뒤, `uptime`으로 서버 상태를 확인해보세요.

## A.

```bash
view users.csv
# 종료는 :q

less big.log
# 종료는 q

uptime
```
