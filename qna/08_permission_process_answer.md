# ✅ 정답 (08_permission_process.md)

## Q. sleep 100 을 & 로 실행하고, ps -ef | grep 으로 PID를 찾아 kill 해보세요.  

## A.

```bash
sleep 100 &
ps -ef | grep sleep
kill <PID>
```

* * *

## Q. log_writer.sh를 nohup 으로 실행해보고, 터미널을 종료/재접속한 뒤에도 살아있는지 확인해보세요.  

## A.

```bash
nohup ./log_writer.sh &
exit    # 세션 종료

# 재접속 후
ps -ef | grep log_writer
tail -f nohup.out
```

* * *

## Q. 같은 작업을 screen 세션 안에서 실행한 뒤, detach/reattach 를 반복해보세요.

## A.

```bash
screen -S logsession
./log_writer.sh

# Ctrl + A, 누른 후 D (Detach)
screen -ls
screen -r logsession

# 프로세스 확인 및 종료
ps -ef | grep log_writer
kill <PID>
```
