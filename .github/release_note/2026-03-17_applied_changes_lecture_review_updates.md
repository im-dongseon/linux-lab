# 📋 2026-03-17 반영 기록 — 검토서 기반 강의자료 수정

> 작성일: 2026-03-17  
> 성격: **이미 반영된 변경 이력 정리** — `.github/lecture_review.md` 기준으로 실제 강의 파일과 일부 QnA를 수정한 내역입니다.

---

## 1. 반영 기준

- 기준 문서: `.github/lecture_review.md`
- 적용 원칙:
  - 강의 흐름은 유지
  - 초보자 혼란을 줄이는 방향으로 문구 보강
  - 실습 의도가 있는 실패 예시는 유지하되, 오타나 모호한 표현은 정리
  - 본문보다 정답 문서에 두는 것이 자연스러운 해설은 QnA와 역할 분리 유지

---

## 2. 파일별 수정 내역

### `02_basic.md`

- `실습 8` 제목을 `man`/`--help` 사용 용도로 명확화
- `sudo unminimize` 실행 시 interactive 안내가 나올 수 있다는 주석 추가
- 불필요한 빈 헤더 제거

### `04_vim_advanced.md`

- `.vimrc`를 왜 홈 디렉토리에 둬야 하는지 설명 보강
- `vim`이 기본적으로 `~/.vimrc`를 읽는다는 점 명시

### `05_file_dir.md`

- `test_foler` 오타를 `test_folder`로 수정
- 디렉토리 복사 실패 예시의 의도를 살리면서 설명을 더 명확하게 정리
- `mv` 실습은 성공 흐름을 먼저 보여주도록 조정
- 디렉토리가 없을 때 발생하는 오류는 코드 블록 아래 설명으로 분리

### `06_network_script.md`

- `curl https://ifconfig.me` 사용으로 정리하고 외부 서비스 의존 주의사항 추가
- 대체 서비스(`https://ipecho.io/plain`) 안내 추가
- 출력 예시(`/root`)를 명령어와 분리해 복붙 혼란 방지
- `chmod +x` 의미를 스크립트 실습에 맞춰 설명 보강
- `check_disk.sh`를 예외 상황에 덜 취약한 형태로 수정
- `sed 's/%//'` 의미 설명 추가
- 추가 실습 힌트 끝부분의 중복 문장과 깨진 코드 블록 정리

### `07_script_advanced.md`

- 위치 매개변수 예시에 `./calc.sh 15 30` 호출 시 `$1`, `$2`에 어떤 값이 들어가는지 설명 추가

### `08_permission_process.md`

- `# TODO: script.sh 생성` 을 학습자용 실제 지시 문장으로 변경
- `chmod +x`, `chmod 755`, `chmod u+x`, `chmod g-w` 설명 추가
- `chown root:root` 의 `사용자:그룹` 의미 설명 추가
- `log_writer.sh` 종료 부분의 `TODO`를 학생 지시형 문장으로 변경
- `screen` detach 키 표기를 `Ctrl + A 를 누른 뒤, 손을 떼고 D` 로 명확화

### `09_practical_cmd.md`

- `echo -e` 의미 설명 추가
- `service + nginx` 실습을 호스트 실행 단계와 컨테이너 내부 실행 단계로 분리
- 코드 블록 내부 프롬프트 문자 제거
- 빈 헤더 제거
- `-p 8080:80` 포트 매핑 설명 보강
- 실습 요약 표의 압축/텍스트 처리 명령 표기를 초보자 친화적으로 구체화

### `qna/06_network_script_answer.md`

- `ifconfig.me` 호출을 HTTPS로 정리
- 외부 서비스 응답 불가 시 대체 URL 안내 추가

---

## 3. 이번 반영의 핵심 효과

1. 복붙 시 바로 실패하는 요소를 줄였다.
2. 초보자가 자주 헷갈리는 권한, 소유권, 포트 매핑, 위치 매개변수 설명을 보강했다.
3. `TODO`처럼 내부 메모로 보일 수 있는 표현을 실제 강의 문장으로 교체했다.
4. 본문과 QnA의 역할 분리를 더 분명히 했다.

---

## 4. 후속 권장 사항

- `.github/comprehensive_summary.md`도 이번 강의 원문 변경 사항에 맞춰 추가 동기화 검토
- `ifconfig.me` 외부 의존 실습은 수업 환경에 따라 별도 공지 문구를 슬라이드/수업 노트에도 포함 권장
- 필요 시 `qna/08_permission_process_answer.md`에 `tail -f` 종료 후 `log_writer.sh` 종료 흐름을 한 줄 더 강조 가능
