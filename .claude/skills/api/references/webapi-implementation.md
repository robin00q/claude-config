# WebAPI (Controller) 구현 가이드

---

**경로**: `adapter/in/webapi/app/{aggregate}/`

## 의존성 선택

---

| 상황 | 의존 대상 |
|------|----------|
| 여러 Aggregate 조율 | Facade |
| 단일 Aggregate CRUD | Query/Mutation 인터페이스 |

## Controller 작성

---

```kotlin
@RestController
@RequestMapping("/api/app/members")
class MemberController(
    private val selectBoxFacade: SelectBoxFacade,  // Facade
    private val memberQuery: MemberQuery           // Query 인터페이스
) {

    @PostMapping("/me/boxes")
    fun addMyBox(
        @AuthenticationPrincipal member: AuthenticatedMember,
        @RequestBody request: AddBoxRequest
    ): ResponseEntity<Unit> {
        selectBoxFacade.selectBox(
            SelectBoxCommand(
                memberId = member.memberId,
                boxId = request.boxId
            )
        )
        return ResponseEntity.ok().build()
    }

    @GetMapping("/me")
    fun getMe(
        @AuthenticationPrincipal member: AuthenticatedMember
    ): ResponseEntity<MemberResponse> {
        val found = memberQuery.findById(member.memberId)
            ?: return ResponseEntity.notFound().build()
        return ResponseEntity.ok(MemberResponse.from(found))
    }
}
```

## Request → Command 변환

---

- Controller에서 생성된 Request DTO → Domain Command 변환
- Enum 변환: `DomainEnum.valueOf(request.type.name)`
- nullable 처리: `request.name ?: ""`

```kotlin
@PostMapping
fun register(
    @RequestBody request: RegisterWorkoutRequest
): ResponseEntity<Long> {
    val command = RegisterWorkoutCommand(
        name = request.name,
        type = WorkoutType.valueOf(request.type.name),  // enum 변환
        description = request.description ?: ""         // nullable 처리
    )
    val id = workoutMutation.register(command)
    return ResponseEntity.ok(id)
}
```

## 인증 정보 주입

---

`@AuthenticationPrincipal`로 인증된 사용자 정보 주입:

```kotlin
@GetMapping("/me")
fun getMe(
    @AuthenticationPrincipal member: AuthenticatedMember
): ResponseEntity<...> {
    // member.memberId 사용
}
```

## 응답 처리

---

- `ResponseEntity`로 HTTP 상태 코드 명시
- 성공: `ResponseEntity.ok(body)`
- 생성: `ResponseEntity.status(HttpStatus.CREATED).body(id)`
- Not Found: `ResponseEntity.notFound().build()`
