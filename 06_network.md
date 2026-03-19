# 🕓 6. 네트워크 실습

## 🎯 목표

리눅스에서 **네트워크 명령어, curl 활용, wget 다운로드**를 익힌다.

---

# 💻 실습

## 🧩 실습 1 — ping 설치 + 네트워크 연결 확인

Ubuntu 최소 이미지에서는 ping 패키지가 없을 수 있으므로 설치가 필요합니다.

```bash
apt update && apt install -y iputils-ping curl
```

테스트:

```bash
ping -c 4 google.com
```

---

## 🧩 실습 2 — curl 기본 사용

```bash
curl https://example.com
curl https://ifconfig.me   # 공인 IP 확인
```

> `ifconfig.me` 는 외부 서비스이므로 네트워크 상태나 실행 환경에 따라 응답하지 않을 수 있습니다.  
> 응답이 없으면 `curl https://ipecho.io/plain` 으로도 확인해보세요.

---

## 🧩 curl 테스트용 사이트 모음

| 사이트 | 용도  | 예시  |
| --- | --- | --- |
| [https://httpbin.org](https://httpbin.org) | GET/POST/헤더/리다이렉트 테스트 | `curl https://httpbin.org/get` |
| [https://postman-echo.com](https://postman-echo.com) | GET/POST 테스트 | `curl https://postman-echo.com/get?foo=bar` |
| [https://ifconfig.me](https://ifconfig.me) | 공인 IP 확인 | `curl https://ifconfig.me` |
| [https://example.com](https://example.com) | 단순 HTML 다운로드 | `curl https://example.com` |

---

## 🧩 curl 유용한 옵션 실습 

### ✔ `-v` : 상세 로그 출력

```bash
curl -v https://httpbin.org/get
```

### ✔ `-I` : 응답 헤더만 가져오기

```bash
curl -I https://example.com
```

### ✔ `-L` : 리다이렉트 따라가기

```bash
curl -L https://httpbin.org/redirect/2
```

### ✔ `-H` + `-d` : 헤더 추가 + POST JSON

```bash
curl -v -X POST https://httpbin.org/post   -H "Content-Type: application/json"   -d '{"name":"linux-user"}'
```

---

## 🧩 curl 다운로드 

example.com 의 index.html 다운로드:

```bash
curl -o index.html https://www.example.com/
```

혹은 URL 그대로 파일명으로 저장하고 싶다면:

```bash
curl -O https://www.example.com/index.html
```

다운로드 확인:

```bash
ls -l
cat index.html
```

---

## 🧩 실습 3 — wget 다운로드 

```bash
apt update && apt install -y wget
wget https://www.example.com/
```

index.html 확인:

```bash
ls -l
cat index.html
```

---

## 📘 실습 요약

| 기능 | 명령어 / 문법 |
|------|-------------|
| 네트워크 연결 확인 | `ping` |
| HTTP 요청 | `curl`, `curl -v/I/L/X` |
| 웹에서 파일 다운로드 | `wget`, `curl -o`, `curl -O` |
