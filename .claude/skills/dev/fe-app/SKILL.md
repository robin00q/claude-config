---
name: fe-app
description: React Native 기능 개발 워크플로우. API 타입 동기화 → 서비스 → 상수 → 화면 → 컴포넌트 → 테스트 → 문서화.
---

**새 기능 개발 시 아래 Phase 순서대로 진행**

# 공통 규칙 로드

---

- `Read("references/common/project-structure.md")` - 프로젝트 구조
- `Read("references/common/design-tokens.md")` - 디자인 토큰 사용법
- `Read("references/common/conventions.md")` - 코드 컨벤션

# Phase 0: API 스펙 동기화

---

1. **서브모듈 업데이트**:
   ```bash
   git submodule update --remote api-spec
   ```

2. **타입 생성**:
   ```bash
   npm run generate:api
   ```

3. **생성된 타입 확인**: `src/types/generated/` (DTO 자동 생성됨)

# Phase 1: API 서비스 구현

---

1. **레퍼런스 로드**: `Read("references/api-service.md")`

2. **작성 대상**:
   - API 호출 함수
   - `src/types/generated/`에서 타입 import

3. **경로**: `src/services/private/{domain}/`

# Phase 2: 상수 정의

---

1. **레퍼런스 로드**: `Read("references/constants.md")`

2. **작성 대상**:
   - 도메인별 라벨 매핑
   - 타입별 설정 객체 (아이콘, 색상 등)

3. **경로**: `src/constants/{domain}.ts`

# Phase 3: 화면 구현

---

1. **레퍼런스 로드**: `Read("references/screen.md")`

2. **작성 대상**:
   - 화면 컴포넌트 (`index.tsx`)
   - 레이아웃 (필요시 `_layout.tsx`)

3. **경로**: `src/app/{route}/`

# Phase 4: 컴포넌트 분리

---

1. **레퍼런스 로드**: `Read("references/component.md")`

2. **분리 기준**:
   - 재사용 가능한 UI → `src/components/{domain}/`
   - 화면 전용 복잡한 로직 → 별도 컴포넌트

# Phase 5: 테스트 작성

---

1. **레퍼런스 로드**: `Read("references/testing.md")`

2. **작성 대상**:
   - 순수 함수 테스트 (별도 파일 분리 필수)
   - 컴포넌트 테스트 (필요시)

3. **경로**: `src/{domain}/__tests__/`

# Phase 6: 문서화

---

**각 디렉토리 CLAUDE.md 작성/업데이트**:

1. **화면 문서**: `Read("references/screen-documenter.md")`
   - 경로: `src/app/{route}/CLAUDE.md`

2. **컴포넌트 문서**: `Read("references/component-documenter.md")`
   - 경로: `src/components/{domain}/CLAUDE.md`

3. **API 서비스 문서**: `Read("references/api-service-documenter.md")`
   - 경로: `src/services/private/{domain}/CLAUDE.md`

# Phase 7: 검증

---

1. **레퍼런스 로드**: `Read("references/checklist.md")`

2. **병렬 실행**:
   ```bash
   npm run type-check
   npm run lint
   npm test
   ```

3. **수동 확인**: 화면 동작, 다크모드, 에러 케이스
