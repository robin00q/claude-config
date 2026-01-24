---
name: spring-be-api-dev
description: 개발 워크플로우 스킬. 코드 구현 → 테스트 → 문서화 순서로 진행.
             Hexagonal Architecture 준수. 새 기능 구현, API 엔드포인트 추가 시 사용.
---

**When developing a new feature, always follow these phases:**

# 공통 규칙 로드

---

- `Read("references/common/hexagonal-architecture.md")` - 아키텍처 다이어그램
- `Read("references/common/error-handling.md")` - 예외/로깅 규칙

# Phase 0: 사전 확인

---

1. **Controller 작성 여부 확인** (AskUserQuestion)
2. **Controller 작성 시**: 서브모듈 로컬 경로 질문 (기본값: `../crossfit-api-spec/`)

# Phase 1: Domain 구현

---

1. **레퍼런스 로드**: `Read("references/domain-implementation.md")`

2. **작성 대상**:
   - Entity (`BaseEntity` 상속, 팩토리 메서드)
   - Value Object
   - Command DTO (`dto/` 하위)
   - Exception (`BusinessException` 상속)

3. **경로**: `domain/{aggregate}/`

# Phase 2: Service 구현

---

1. **레퍼런스 로드**: `Read("references/service-implementation.md")`

2. **작성 대상**:
   - `{Aggregate}Mutation` 인터페이스
   - `{Aggregate}Query` 인터페이스
   - `{Aggregate}Repository` 인터페이스
   - `{Aggregate}Service` 구현체

3. **경로**: `application/service/{aggregate}/`

# Phase 3: Facade 구현 (필요시)

---

1. **레퍼런스 로드**: `Read("references/facade-implementation.md")`

2. **사용 조건**: 여러 Aggregate 조율 필요 시에만

3. **작성 대상**:
   - `{BusinessIntent}Facade`
   - Input/Output nested class

4. **경로**: `application/facade/{domain}/`

# Phase 4: API Spec (Phase 0에서 선택 시)

---

1. api-spec 스킬 호출: `Skill("api-spec")`
2. 서브모듈 최신화: `git submodule update --remote api-spec`

# Phase 5: Controller 구현 (Phase 0에서 선택 시)

---

1. **레퍼런스 로드**: `Read("references/webapi-implementation.md")`

2. **작성 대상**:
   - `{Aggregate}Controller`
   - Request → Command 변환 로직

3. **경로**: `adapter/in/webapi/app/{aggregate}/`

# Phase 6: Fixture 작성

---

1. **레퍼런스 로드**: `Read("references/fixtures.md")`

2. **작성 대상**:
   - `{Entity}Fixtures.kt`
   - `{Command}Fixtures.kt`

3. **경로**: `src/testFixtures/kotlin/crossfit/domain/{aggregate}/`

# Phase 7: 테스트 작성

---

1. **레퍼런스 로드**: `Read("references/testing.md")`

2. **작성 대상**:

   | 대상 | 어노테이션 | 위치 |
   |------|-----------|------|
   | Domain | 없음 | `test/.../domain/{aggregate}/` |
   | Service | `@SpringBootTest` | `test/.../application/service/{aggregate}/provided/` |
   | Facade | `@SpringBootTest` | `test/.../application/facade/{domain}/` |
   | Controller | `@WebMvcTest` | `test/.../adapter/in/webapi/app/{aggregate}/` |

# Phase 8: 문서화

---

**병렬 실행** (해당 계층만):
- `Read("references/domain-documenter.md")` → `domain/{aggregate}/CLAUDE.md`
- `Read("references/service-documenter.md")` → `application/service/{aggregate}/CLAUDE.md`
- `Read("references/facade-documenter.md")` → `application/facade/{domain}/CLAUDE.md`

# Phase 9: 완료 검증

---

**병렬 실행**:
- `Read("references/checklist.md")` 후 확인
- `./gradlew build`
