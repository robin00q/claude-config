# TestFixtures 작성 가이드

---

## 기본 정보

**위치**: `src/testFixtures/kotlin/wodly/domain/{aggregate}/`
**패턴**: Object Mother

---

## 디렉토리 구조

```
src/testFixtures/kotlin/wodly/
└── domain/
    └── {aggregate}/
        ├── {Entity}Fixtures.kt
        └── {Command}Fixtures.kt
```

---

## 기본 구조

```kotlin
object {Entity}Fixtures {

    // 1. 기본 생성 메서드 (모든 파라미터에 기본값)
    fun create(
        field1: Type = defaultValue1,
        field2: Type = defaultValue2
    ): Entity {
        val command = RegisterCommand(...)
        return Entity.register(command)
    }

    // 2. 시나리오별 변형 메서드 (자주 사용되는 조합만)
    fun forScenarioA(...): Entity = create(...)
}
```

---

## 명명 규칙

| 메서드 유형 | 네이밍 | 예시 |
|------------|--------|------|
| 기본 생성 | `create()` | `create(memberId = 1L)` |
| 시나리오 변형 | `for{Scenario}()` | `forTimeRecord()` |
| 조건 변형 | `with{Condition}()` | `withScaledWeight()` |

---

## 작성 원칙

### 1. 기본값은 유효한 최소 객체

```kotlin
// Good: 파라미터 없이 호출해도 유효한 객체 생성
val record = IndividualWorkoutRecordFixtures.create()

// Bad: 필수 파라미터 요구
val record = IndividualWorkoutRecordFixtures.create(memberId, workoutId, ...)
```

### 2. 테스트 의도가 드러나는 파라미터만 전달

```kotlin
// Good: 테스트에 중요한 값만 명시
val record = IndividualWorkoutRecordFixtures.create(memberId = 123L)

// Bad: 불필요한 값까지 전달
val record = IndividualWorkoutRecordFixtures.create(
    memberId = 123L,
    gender = Gender.MALE,  // 테스트와 무관
    workoutId = 1L         // 테스트와 무관
)
```

### 3. 시나리오 메서드는 자주 사용되는 조합만

```kotlin
// Good: 반복되는 시나리오
fun forTimeRecord(finishTime: Time = Time(5, 30)) = create(...)
fun amrapRecord(reps: Int = 100) = create(...)

// Bad: 일회성 조합
fun forMember123WithWorkout456AndScaledWeight() = ...
```

### 4. Command Fixtures는 별도 클래스로 분리

```kotlin
object RegisterBoxCommandFixtures {
    fun create(
        name: String = "CrossFit Seoul",
        sido: String = "서울특별시",
        sigungu: String = "강남구",
        detail: String = "테헤란로 123"
    ): RegisterBoxCommand = RegisterBoxCommand(
        name = name,
        sido = sido,
        sigungu = sigungu,
        detail = detail
    )
}

object BoxFixtures {
    fun create(
        name: String = "CrossFit Seoul",
        sido: String = "서울특별시",
        sigungu: String = "강남구",
        detail: String = "테헤란로 123"
    ): Box {
        val command = RegisterBoxCommandFixtures.create(
            name = name,
            sido = sido,
            sigungu = sigungu,
            detail = detail
        )
        return Box.register(command)
    }
}
```
