# ✅ 정답 (07_shell_script.md)

## Q. ifconfig.me를 활용해 자신의 공인 IP를 확인하고, 해당 결과를 my_ip.txt 파일로 저장하는 쉘 스크립트(get_ip.sh)를 작성해보세요.

## A.

```bash
cat << 'EOF' > get_ip.sh
#!/bin/bash
curl https://ifconfig.me > my_ip.txt
echo "IP saved to my_ip.txt"
EOF

chmod +x get_ip.sh
./get_ip.sh
```

> `ifconfig.me` 가 응답하지 않으면 `curl https://ipecho.io/plain > my_ip.txt` 로도 대체할 수 있습니다.
