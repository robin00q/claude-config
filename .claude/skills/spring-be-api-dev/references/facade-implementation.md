# Facade 구현 가이드

---

**경로**: `application/facade/{domain}/`

## 사용 조건

---

**여러 Aggregate 조율이 필요할 때만 생성**

| 상황 | 선택 |
|------|------|
| 단일 Aggregate CRUD | Service 직접 사용 |
| 여러 Aggregate 조율 | Facade 생성 |

## 핵심 규칙

---

1. **Query/Mutation 인터페이스만 의존**
2. **Repository 직접 의존 금지**
3. **Service 구현체 직접 의존 금지**
4. **Facade 간 의존 금지**

## Facade 작성

---

**명명**: `{BusinessIntent}Facade`

```kotlin
@Service
@Transactional
class SelectBoxFacade(
    private val boxQuery: BoxQuery,            // Query 인터페이스
    private val memberMutation: MemberMutation // Mutation 인터페이스
) {
    fun selectBox(command: SelectBoxCommand) {
        // 1. 다른 Aggregate 검증
        require(boxQuery.existsById(command.boxId)) {
            "Box not found: ${command.boxId}"
        }
        // 2. Mutation 호출
        memberMutation.addBoxMember(command.memberId, command.boxId)
    }
}
```

## Input/Output 규칙

---

### Input

- **도메인 Command 사용 (권장)**: Controller에서 완전히 생성 가능한 경우
- **Facade Input 사용**: 다른 도메인 조회 후 값을 채워야 하는 경우

### Output

여러 값 반환 시 nested class 정의:

```kotlin
@Service
class LoginFacade(...) {

    fun login(verificationId: Long): Output {
        // ...
        return Output(memberId, accessToken, refreshToken)
    }

    data class Output(
        val memberId: Long,
        val accessToken: String,
        val refreshToken: String
    )
}
```

### 명명 규칙

| 유형 | Input | Output |
|------|-------|--------|
| 단일 메서드 | `Input` | `Output` |
| 다중 메서드 | `RegisterInput` | `RegisterOutput` |
