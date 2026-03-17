# 📋 2026-03-14 반영 기록 — 파일명, 제목, QnA 정리

> 작성일: 2026-03-14  
> 성격: **이미 반영된 변경 이력 정리** — 본 문서는 파일명, 내부 제목, QnA 구조 정리 내역을 기록합니다.

---

## 1. 공통 규칙 (기초 구조 + 커리큘럼 정리 내용 종합)

전체 강의를 관통하는 필수 규칙입니다. 향후 새로운 파일을 만들거나 수정할 때 항상 아래 규칙을 준수해야 합니다.

1. **파일 보호**: 원본 데이터(강의용 `.md` 파일들)는 요구사항 분석과 계획(plan 문서) 수립이 확정되기 전까지 절대 직접 수정하지 않습니다.
2. **단락 구성 포맷 (표준안)**:
   - `## 🎯 목표` (필수)
   - `## 🧩 개요` (선택)
   - `## 📁 사전 준비` (선택, 실습 스크립트 생성 등)
   - `## 🧩 실습 N` (필수, 반복)
   - `## 📘 실습 요약` (필수, 명령어 표 정리)
   - `## 🎯 추가 실습` (필수, 답안은 qna 폴더에 분할 저장)
3. **경로 및 마운트 규칙**:
   - 로컬 볼륨 마운트 경로는 항상 `~/workspace/docker/linux-lab` 를 사용합니다.
   - `docker run -v` 사용 전, macOS 권한 오류 방지를 위해 항상 사전에 `mkdir -p ~/workspace/docker/linux-lab` 를 안내합니다.
4. **qna 폴더 구조화**:
   - 통합된 `99_QnA.md` 대신, `qna/` 디렉토리 아래에 각 챕터와 매칭되는 `[챕터명]_answer.md` 형태로 정답을 분할 보관합니다.

---

## 2. 파일 제목(넘버링) 및 오타 수정 계획

파일 이름은 정상적으로 `01`부터 `10`번까지 리네임되었으나, 파일 내부 최상단의 헤더(`# ...`) 제목에는 예전 넘버링과 오타가 남아있어 이를 일괄 수정합니다.

### 2.1 본문 파일 제목 업데이트 ✅ 완료
- `01_intro.md` : `# 🌱 1. 리눅스란 무엇인가?` (유지)
- `02_basic.md` : `# 🕑 2. 리눅스 기본 명령어 실습`
- `03_vim_basic.md` : `# 🕐 3. 리눅스 텍스트 편집기 vi/vim 기본` (기존 2-1)
- `04_vim_advanced.md` : `# 🕐 4. 리눅스 텍스트 편집기 vi/vim 심화` (기존 2-2)
- `05_file_dir.md` : `# 🕑 5. 파일·디렉토리 다루기 실습` (기존 3)
- `06_network_script.md` : `# 🕓 6. 네트워크 & 쉘 스크립트 실습` (기존 4)
- `07_script_advanced.md` : `# 🕓 7. 쉘 스크립트 심화` (기존 4-1)
- `08_permission_process.md` : `# 🕒 8. 권한, 프로세스, screen 실습` (기존 5)
- `09_practical_cmd.md` : `# 🕓 9. 실무에서 자주 쓰는 명령어 실습` (기존 6)
- `10_reference.md` : `# 🕓 10. 참고` (기존 7)

### 2.2 QnA 파일 제목 업데이트 (`qna/` 폴더) ✅ 완료
답안 파일의 내용 중 원본 파일명을 가리키는 제목도 모두 최신 파일명으로 일괄 교체합니다.
- `02_basic_answer.md`: `# ✅ 정답 (02_basic.md)`
- `03_vim_basic_answer.md`: `# ✅ 정답 (03_vim_basic.md)` (기존 02_1_vim_answer.md)
- `04_vim_advanced_answer.md`: `# ✅ 정답 (04_vim_advanced.md)` (기존 02_2_vim_answer.md)
- `05_file_dir_answer.md`: `# ✅ 정답 (05_file_dir.md)` (기존 03_answer.md)
- `06_network_script_answer.md`: `# ✅ 정답 (06_network_script.md)` (기존 04_answer.md)
- `07_script_advanced_answer.md`: `# ✅ 정답 (07_script_advanced.md)` (기존 04_1_answer.md)
- `08_permission_process_answer.md`: `# ✅ 정답 (08_permission_process.md)` (기존 05_answer.md)
- `09_practical_cmd_answer.md`: `# ✅ 정답 (09_practical_cmd.md)` (기존 06_answer.md)

### 2.3 발견된 주요 오타 수정 내역 ✅ 완료
- `01_intro.md` (L77): `timezone 설정이 필요하면` → `timezone 설정이 필요하면` (한글 자모 분리 현상 수정)
- `08_permission_process.md`: 주석 내용 중 `생성` → `생성` 수정

---
