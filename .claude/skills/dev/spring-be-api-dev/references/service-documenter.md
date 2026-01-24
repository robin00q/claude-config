# Service 문서화 가이드

---

Application Service 계층 CLAUDE.md 문서 생성/리팩토링 가이드

## 핵심 원칙

---

- **기능 중심**: "무엇을 하는가"에 집중
- **간결함**: 핵심 정보만, 불필요한 수식어 제거
- **맥락 보존**: 이해에 필요한 정보는 생략 금지

## 포함 대상

---

- 서비스 기능 설명 (1-2줄)
- 구현 인터페이스 (이름만)
- 핵심 유스케이스

## 제거 대상

---

- 구현 예시 코드 (`@Service`, `@Transactional` 등)
- 메서드 시그니처 목록
- Repository 상세 정보
- 테스트 섹션

## 문서 템플릿

---

```markdown
# {Aggregate} Application Service

---

{Aggregate}에 대한 유스케이스 조율

## {Service명}

---

### 기능

---

{제공하는 핵심 기능 1-2줄}

### 구현 인터페이스

---

- `{Aggregate}Query`: 조회
- `{Aggregate}Mutation`: 상태 변경
```

## 변환 예시

---

### ❌ 장황한 문서

```markdown
### MemberService 구현

MemberService는 회원과 관련된 모든 비즈니스 로직을 담당합니다.

#### 의존성
@Service
class MemberService(...)

#### 주요 메서드
- `register(command: RegisterMemberCommand): Long`
- `findById(id: Long): Member?`
```

### ✅ 간결한 문서

```markdown
## MemberService

---

### 기능

---

회원 등록 및 조회

### 구현 인터페이스

---

- `MemberQuery`: 조회
- `MemberMutation`: 상태 변경
```
