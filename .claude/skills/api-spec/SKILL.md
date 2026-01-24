---
name: api-spec
description: OpenAPI 스펙 작성 스킬. api-spec 서브모듈에서 Request/Response 스키마 작성,
             커밋, 푸시.
---

**서브모듈 경로는 호출 시 전달받음 (spring-backend-dev에서 질문)**

# 공통 규칙 로드 (필수)

---

**⚠️ Step 진행 전 반드시 읽을 것. 공통 규칙을 읽지 않으면 다음 Step 진행 불가.**

- `Read("references/naming-conventions.md")` - 네이밍 규칙
- `Read("references/common-schemas.md")` - 공용 스키마 참조
- `Read("references/file-structure.md")` - 파일 구조
- `Read("references/yaml-template.md")` - YAML 템플릿
- `Read("references/type-mapping.md")` - 타입 매핑

# Phase 1: 재사용 가능한 공통 스키마 확인 및 없는 경우 추출

---

1. `{submodule-path}/specs/common/openapi.yaml` 확인
2. 필요한 공통 스키마가 없으면 추가

# Phase 2: OpenAPI yaml 작성

---

1. **경로 확인**: `{submodule-path}/specs/{domain}/openapi.yaml`
2. **신규 도메인**: 파일 생성
3. **기존 도메인**: 파일 수정 (paths, schemas 추가)

# Phase 3: 커밋 & 푸시

---

```bash
git -C {submodule-path} add .
git -C {submodule-path} commit -m "feat: {domain} API 스펙 추가"
git -C {submodule-path} push
```
