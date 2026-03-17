# 수정 계획 및 릴리즈 노트 (Release Note)

최근 강의 데이터 및 사용자 요청에 따라 다음과 같은 수정 및 개선이 진행됩니다.

## 1. 개요
- **목적**: 파일 길이가 길어진 06장 실습 내용을 분리하여 학습 주제를 명확히 하고, 초보자를 위한 쉘/터미널 기본 개념 설명을 보강합니다.
- **주요 변경 사항**: 네트워크 실습과 쉘 스크립트 실습 분리, 관련 파일 넘버링 전체 자동 재조정 등.

## 2. 세부 변경 사항

### 2.1 문서 분리 및 내용 추가
- **`06_network_script.md` 파일 분리**:
  - `06_network.md`: 네트워크 실습(ping, curl, wget 등) 전담 챕터로 유지.
  - `07_shell_script.md`: 기초 쉘 스크립트(환경 변수, 기초 문법, 조건/반복문) 전담 챕터로 분리.
- **터미널과 쉘의 개념 차이 설명 추가** (`07_shell_script.md` 내):
  - iTerm2, Windows Terminal 등 "터미널 에뮬레이터"와 bash, zsh 같은 "쉘"이 어떻게 다른지 초보자 관점에서 쉽게 설명 기입.
  - bash, zsh 등 주요 쉘의 개요와 실습 전 알아야 할 기초 내용 보강.

### 2.2 후속 파일 넘버링 및 내부 헤더 동기화
`06`장에서 `07`장이 새롭게 파생됨에 따라 기존 07장 이후의 모든 강의 파일과 Q&A 파일의 번호를 **+1 증가**시킵니다.
또한 마크다운 파일 내부의 첫 번째 헤드라인(`. # 🕓 X. ` 형식)의 번호도 파일명에 맞추어 일괄적으로 변경합니다.

- **본문 파일 변경**:
  - `07_script_advanced.md` ➔ `08_script_advanced.md`
  - `08_permission_process.md` ➔ `09_permission_process.md`
  - `09_practical_cmd.md` ➔ `10_practical_cmd.md`
  - `10_reference.md` ➔ `11_reference.md`
- **정답 (QnA) 파일 변경**:
  - `qna/06_network_script_answer.md` ➔ `qna/07_shell_script_answer.md` (스크립트 정답 포함 파일이므로 이동)
  - `qna/07_script_advanced_answer.md` ➔ `qna/08_script_advanced_answer.md`
  - `qna/08_permission_process_answer.md` ➔ `qna/09_permission_process_answer.md`
  - `qna/09_practical_cmd_answer.md` ➔ `qna/10_practical_cmd_answer.md`

이 계획에 따라 기존 파일들의 백업 및 변경 처리를 진행합니다. 
