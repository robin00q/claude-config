---
name: iac
description: Terraform 인프라 개발 워크플로우. 리소스 설계 → 비용 확인 → 구현 → 검증 → 문서화 → 적용 안내.
             AWS Lightsail 기반 인프라 관리. 숙련된 Terraform 엔지니어 관점으로 작업.
---

**인프라 변경 시 아래 Phase 순서대로 진행:**

**⚠️ 요청된 작업에 해당하는 Phase부터 시작. (예: "변수만 추가해주세요" → Phase 3부터)**

# 보안 규칙 (필수)

---

**⚠️ 모든 Phase에서 반드시 준수. 보안에 매우 민감한 프로젝트.**

- **포트**: 필요한 포트만 최소 개방. SSH(22)는 키 인증만 허용 (브라우저 SSH + CI/CD용)
- **접근 제어**: DB `publicly_accessible = false`, 내부 통신만 허용
- **민감 정보**: 비밀번호/시크릿은 반드시 `sensitive = true` 변수로 분리
- **커밋 금지**: `terraform.tfvars`, `*.tfstate` 절대 git 커밋 금지
- **보안 영향 검토**: 포트 추가, 접근 범위 확대 시 사용자에게 보안 리스크 반드시 안내

# Phase 0: 사전 확인

---

1. **요구사항 확인** (AskUserQuestion)
   - 추가/변경할 리소스 종류
   - 환경 (dev/prod)
   - 비용 제약사항

2. **작업 디렉토리**: `wodly-iac/`

# Phase 1: 설계

---

1. **레퍼런스 로드**: `Read("references/terraform-conventions.md")`

2. **설계 항목**:
   - 필요한 리소스 목록
   - 리소스 간 의존성
   - 변수 및 출력값 정의

# Phase 2: 비용 안내 (필수)

---

**⚠️ 이 단계를 건너뛰지 마세요. 구현 전 반드시 비용 변화를 사용자에게 안내.**

1. **리소스 가격은 반드시 최신 공식 문서를 WebSearch로 검색하여 확인 후 명시.**
2. 리소스별 변경 내역과 비용 영향을 표로 안내:

| 리소스 | 변경 | 월 비용 |
|--------|------|---------|
| Lightsail Instance | **추가** | +$5 |
| Lightsail DB | **삭제** | -$15 |
| Static IP | 변경 없음 | $0 |
| **합계** | | **-$10** |

2. 사용자 확인 후 다음 Phase 진행

# Phase 3: 구현

---

1. **파일 작성/수정**:
   - `main.tf` — provider, 리소스 정의
   - `variables.tf` — 변수 선언
   - `outputs.tf` — 출력값
   - `terraform.tfvars` — 실제 값 (`.gitignore` 대상)

2. **규칙**:
   - 레퍼런스의 네이밍/구조 규칙 준수
   - 민감 정보는 변수로 분리

# Phase 4: 검증 (필수)

---

**⚠️ 이 단계를 건너뛰지 마세요. 검증 없이 완료 처리 금지.**

1. **레퍼런스 로드**: `Read("references/checklist.md")`
2. `terraform fmt` — 포맷팅
3. `terraform validate` — 문법 검증
4. `terraform plan` — 변경사항 미리보기

# Phase 5: 문서화 (필수)

---

**⚠️ 이 단계를 건너뛰지 마세요. 코드 수정 시 관련 CLAUDE.md 반드시 업데이트.**

1. **레퍼런스 로드**: `Read("references/documenter.md")`

2. **작성 대상**:
   - 변경된 리소스가 위치한 디렉토리의 CLAUDE.md
   - 새 디렉토리 생성 시 해당 디렉토리에 CLAUDE.md 필수 생성

# Phase 6: 적용 안내

---

사용자가 직접 실행할 명령어를 안내:

```bash
cd wodly-iac
terraform init    # 최초 1회 또는 provider 변경 시
terraform apply   # 인프라 적용
```

- 변경 대상 리소스 요약
- 주의사항 (데이터 손실 가능성, 다운타임 등)

# Phase 7: 코드리뷰

---

1. **레퍼런스 로드**: `Read("references/code-review.md")`
2. `git diff main...HEAD --name-only` 로 변경 파일 목록 확보
3. 레퍼런스의 프롬프트 템플릿에 파일 목록을 채워서 Task 도구로 코드리뷰 에이전트 실행 (`subagent_type=general-purpose`)
4. 리뷰 결과를 사용자에게 전달
