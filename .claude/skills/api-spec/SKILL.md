---
name: api-spec
description: OpenAPI 스펙 작성 스킬. api-spec 서브모듈에서 Request/Response 스키마 작성,
             커밋, 푸시.
---

**서브모듈 경로는 호출 시 전달받음 (spring-backend-dev에서 질문)**

# Step 1: 레퍼런스 로드

---

`Read("references/openapi-guide.md")`

# Step 2: OpenAPI yaml 작성

---

1. **경로 확인**: `{submodule-path}/specs/{domain}/openapi.yaml`
2. **신규 도메인**: 파일 생성
3. **기존 도메인**: 파일 수정 (paths, schemas 추가)

# Step 3: 서브모듈 커밋 & 푸시

---

```bash
cd {submodule-path}
git add .
git commit -m "feat: {domain} API 스펙 추가"
git push
```
