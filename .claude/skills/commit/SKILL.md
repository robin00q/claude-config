---
name: commit
description: 특정 디렉토리의 변경사항을 커밋. 디렉토리 경로를 인자로 전달.
---

# Step 1: 변경사항 확인

---

1. 인자로 받은 디렉토리 경로 확인
2. 없으면 AskUserQuestion으로 질문 (**multiSelect: true** 로 여러 디렉토리 선택 가능)
3. 선택된 각 디렉토리의 git status 확인
4. staged/unstaged 변경사항 출력

# Step 2-4: 각 디렉토리별 반복 실행

---

선택된 디렉토리마다 아래 순서로 진행:

## 스테이징

1. 해당 디렉토리 하위 변경 파일만 `git add`
2. 민감 파일(.env 등) 제외 확인

## 커밋

1. 변경사항 분석하여 커밋 메시지 작성
2. CLAUDE.md 커밋 규칙 준수 (한글, 타입 prefix)
3. `git commit` 실행

## 푸시 여부 확인

1. AskUserQuestion으로 푸시 여부 질문
2. 승인 시 `git push` 실행
