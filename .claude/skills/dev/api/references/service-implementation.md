# Service 구현 가이드

---

**경로**: `application/service/{aggregate}/`

## 디렉토리 구조

---

```
{aggregate}/
├── {Aggregate}Service.kt
├── provided/
│   ├── {Aggregate}Mutation.kt
│   └── {Aggregate}Query.kt
└── required/
    └── {Aggregate}Repository.kt
```

## Provided Port: Mutation 인터페이스

---

**위치**: `provided/{Aggregate}Mutation.kt`

```kotlin
interface WorkoutMutation {
    fun register(command: RegisterWorkoutCommand): Long
    fun update(id: Long, command: UpdateWorkoutCommand)
    fun delete(id: Long)
}
```

## Provided Port: Query 인터페이스

---

**위치**: `provided/{Aggregate}Query.kt`

```kotlin
interface WorkoutQuery {
    fun findById(id: Long): Workout?
    fun findAll(): List<Workout>
    fun existsById(id: Long): Boolean
}
```

## Required Port: Repository 인터페이스

---

**위치**: `required/{Aggregate}Repository.kt`

```kotlin
interface WorkoutRepository : JpaRepository<Workout, Long> {
    fun findByDate(date: LocalDate): List<Workout>
}
```

## Service 구현체

---

**위치**: `{Aggregate}Service.kt`

```kotlin
@Service
class WorkoutService(
    private val workoutRepository: WorkoutRepository
) : WorkoutMutation, WorkoutQuery {

    @Transactional
    override fun register(command: RegisterWorkoutCommand): Long {
        val workout = Workout.register(command)
        val saved = workoutRepository.save(workout)
        return saved.id!!
    }

    @Transactional
    override fun update(id: Long, command: UpdateWorkoutCommand) {
        val workout = workoutRepository.findById(id).orElseThrow()
        workout.update(command)
        workoutRepository.save(workout)  // 명시적 save 필수
    }

    @Transactional(readOnly = true)
    override fun findById(id: Long): Workout? {
        return workoutRepository.findById(id).orElse(null)
    }
}
```

## 핵심 규칙

---

1. **단일 Aggregate만 처리** (다른 Aggregate 조율 → Facade)
2. **엔티티 수정 후 명시적 `save()` 호출 필수**
3. **도메인 로직은 Entity에 위임**
4. **CQRS 준수**: Mutation과 Query 분리
