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

## Read Facade 패턴

---

**OSIV 비활성화 환경에서 lazy 컬렉션 포함 ReadModel 반환이 필요할 때 사용**

### 사용 조건

- Controller에서 엔티티 대신 읽기 전용 DTO(ReadModel) 필요
- `@Transactional` 내에서 lazy 컬렉션 접근 + ReadModel 매핑 필요

### 패턴

- **명명**: `Get{Aggregate}Facade`
- **ReadModel**: 같은 패키지에 `{Aggregate}ReadModel` data class 정의
- **어노테이션**: `@Service`, `@Transactional(readOnly = true)`
- **의존성**: Query 인터페이스만 의존

```kotlin
// ReadModel 정의
data class WorkoutSpecReadModel(
    val id: Long,
    val name: String,
    val recordSpecs: List<RecordSpecReadModel>  // lazy 컬렉션 매핑
)

// Read Facade
@Service
@Transactional(readOnly = true)
class GetWorkoutSpecFacade(
    private val workoutSpecQuery: WorkoutSpecQuery
) {
    fun findById(id: Long): WorkoutSpecReadModel? {
        return workoutSpecQuery.findById(id)?.toReadModel()
    }

    private fun WorkoutSpec.toReadModel() = WorkoutSpecReadModel(
        id = this.id!!,
        name = this.name,
        recordSpecs = this.recordSpecs.map { ... }  // lazy 접근 안전
    )
}
```

### 핵심 원칙

- Query는 도메인 엔티티 반환 (ReadModel 변환은 Facade 담당)
- 기존 Facade(`@Transactional`)에서 호출되는 경우 lazy 접근 안전 → Read Facade 불필요
- Read Facade는 Controller에서 직접 읽기 조회할 때만 사용
