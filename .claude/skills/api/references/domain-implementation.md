# Domain 구현 가이드

---

**경로**: `domain/{aggregate}/`

## 디렉토리 구조

---

```
{aggregate}/
├── CLAUDE.md           # 도메인별 문서 (필수)
├── {Entity}.kt
├── {ValueObject}.kt
├── dto/
│   └── *Command.kt
└── exception/
    └── *Exception.kt
```

## Entity 작성

---

- `BaseEntity` 상속
- 팩토리 메서드: `companion object { fun register() }`
- `init` 블록에서 유효성 검증

```kotlin
@Entity
@Table(name = "boxes")
class Box(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long? = null,

    val name: String
) : BaseEntity() {

    init {
        require(name.isNotBlank()) { "name은 공백일 수 없습니다" }
    }

    companion object {
        fun register(command: RegisterBoxCommand): Box {
            return Box(name = command.name)
        }
    }
}
```

## Value Object 작성

---

### Embeddable

```kotlin
@Embeddable
class Timecap(
    val minutes: Int,
    val seconds: Int
) {
    init {
        require(minutes >= 0) { "minutes는 0 이상이어야 합니다" }
    }
}
```

### Enum

```kotlin
enum class WorkoutType {
    EMOM, AMRAP, FOR_TIME
}
```

## Command DTO 작성

---

- **위치**: `domain/{aggregate}/dto/`
- **명명**: `{Action}{Entity}Command`

```kotlin
data class RegisterBoxCommand(
    val name: String,
    val sido: String,
    val sigungu: String,
    val detail: String
)
```

## DDL 컨벤션

---

### 컬럼 순서

`id` → `created_at` → `updated_at` → 나머지 컬럼

### 타입 매핑

| Kotlin | MySQL |
|--------|-------|
| Long (PK/FK) | `BIGINT UNSIGNED` |
| LocalDateTime | `DATETIME` |
| Boolean | `TINYINT(1)` |
| String | `VARCHAR(255)` |
| String (@Lob/TEXT) | `TEXT` |
| LocalDate | `DATE` |
| Int | `INT` |
| Double | `DOUBLE` |

### 네이밍 규칙

| 구분 | 형식 | 예시 |
|------|------|------|
| 인덱스 | `idx_{컬럼1}_{컬럼2}` | `idx_member_id_workout_date` |
| 유니크 | `uk_{컬럼1}_{컬럼2}` | `uk_workout_id_member_id` |

### 기타

- FK constraint 생략 (JPA 연관관계만 유지)
- Boolean: `columnDefinition = "TINYINT(1)"` 명시
- LocalDateTime: `columnDefinition = "DATETIME"` 명시

## Exception 작성

---

- `BusinessException` 상속
- **위치**: `domain/{aggregate}/exception/`
- 로깅 후 throw (ID는 로그에만, 예외 메시지엔 제외)

```kotlin
class MemberAlreadyExistsException : BusinessException(
    message = "이미 존재하는 회원입니다",
    errorType = "/errors/member/already-exists",
    errorTitle = "Member Already Exists",
    httpStatus = HttpStatus.CONFLICT
)
```
