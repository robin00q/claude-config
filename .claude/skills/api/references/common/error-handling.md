# 예외 및 에러처리

---

모든 Phase에서 적용되는 규칙

## 로깅 위치

---

예외 throw 전 로깅:

```kotlin
log.error("권한 없는 기록 수정 시도 - recordId: {}, 소유자: {}, 요청자: {}", ...)
throw WorkoutRecordUnauthorizedException()
```

## 로깅 레벨 선택

---

| 레벨 | 기준 | 예시 |
|------|------|------|
| **ERROR** | 정상적으로 발생할 수 없는 예외 | 권한 없는 접근, UI에 표시된 리소스 미존재 |
| **WARNING** | 보안 모니터링 필요 | 반복 실패로 인한 잠금 (브루트포스) |
| **INFO** | 사용자 행동으로 발생 가능 | 중복 등록, 입력 오류, 만료된 요청 |

## 예외 메시지 작성

---

- **예외 메시지**: 일반적인 설명만 (ID 포함 금지)
- **로그**: 모니터링용 상세 정보 (ID, 컨텍스트 포함)

```kotlin
// 올바른 예시
log.error("운동 기록 조회 실패 - workoutRecordId: {}", workoutRecordId)
throw WorkoutRecordNotFoundException()

// 잘못된 예시
throw WorkoutRecordNotFoundException(workoutRecordId)
```
