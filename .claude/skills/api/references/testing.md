# 테스트 작성 가이드

---

## 공통 규칙

- 테스트명 한글 작성
- Fixtures 사용 (`src/testFixtures/`)
- Given-When-Then 패턴

---

## Domain 테스트

**위치**: `test/.../domain/{aggregate}/`
**스타일**: Kotest `FunSpec`
**어노테이션**: 없음

```kotlin
class BoxTest : FunSpec({
    test("register creates Box") {
        val box = BoxFixtures.create()
        box.name shouldBe "CrossFit Seoul"
    }

    test("fails when name is blank") {
        shouldThrow<IllegalArgumentException> {
            BoxFixtures.create(name = "")
        }
    }
})
```

---

## Service 테스트

**위치**: `test/.../application/service/{aggregate}/provided/`
**스타일**: Kotest `FunSpec`
**어노테이션**: `@SpringBootTest`, `@Transactional`

```kotlin
@SpringBootTest
@Transactional
class BoxMutationTest(
    private val boxMutation: BoxMutation,
    private val boxRepository: BoxRepository
) : FunSpec({
    extension(SpringExtension)

    test("register box") {
        // Given
        val command = RegisterBoxCommand(...)

        // When
        val boxId = boxMutation.register(command)

        // Then
        val saved = boxRepository.findById(boxId).get()
        saved.name shouldBe "CrossFit Seoul"
    }
})
```

---

## Facade 테스트

**위치**: `test/.../application/facade/{domain}/`
**스타일**: Kotest `FunSpec`
**어노테이션**: `@SpringBootTest`, `@Transactional`

```kotlin
@SpringBootTest
@Transactional
class LoginFacadeTest(
    private val loginFacade: LoginFacade,
    private val verificationMutation: VerificationMutation,
    private val memberMutation: MemberMutation,
    private val verificationRepository: VerificationRepository
) : FunSpec({
    extension(SpringExtension)

    test("login succeeds with valid verification and registered member") {
        // Given: 회원 등록
        val phoneNumber = PhoneNumber("010-1111-1111")
        memberMutation.register(RegisterMemberCommand(phoneNumber = phoneNumber, name = "테스트"))

        // Given: 인증 완료
        val verificationId = createVerifiedVerification("010-1111-1111")

        // When
        val result = loginFacade.login(verificationId, VerificationPurpose.LOGIN)

        // Then
        result.accessToken.shouldNotBeEmpty()
        result.refreshToken.shouldNotBeEmpty()
    }

    test("login fails when member not registered") {
        val verificationId = createVerifiedVerification("010-3333-3333")

        shouldThrow<IllegalArgumentException> {
            loginFacade.login(verificationId, VerificationPurpose.LOGIN)
        }
    }
})
```

---

## Controller 테스트

**위치**: `test/.../adapter/in/webapi/app/{aggregate}/`
**스타일**: JUnit5 `@Test` + mockito-kotlin
**명명**: `{Controller명}WebMvcTest`

### 어노테이션 선택

| 상황 | 어노테이션 |
|------|-----------|
| 인증 필요 (`@AuthenticationPrincipal`) | `@SecuredWebMvcTest` |
| 인증 불필요 (permitAll) | `@WebMvcTest` |

### @WebMvcTest 예시 (인증 불필요)

```kotlin
@WebMvcTest(BoxController::class)
class BoxControllerWebMvcTest {

    @Autowired
    lateinit var mvcTester: MockMvcTester

    @MockitoBean
    lateinit var boxQuery: BoxQuery

    @MockitoBean
    lateinit var tokenQuery: TokenQuery  // Security 필터용 (필수)

    @Test
    fun `getBoxes returns boxes by sido`() {
        // Given
        val boxes = listOf(
            createBox(1L, "CrossFit Seoul", "서울특별시", "강남구")
        )
        whenever(boxQuery.findBySido("서울특별시")).thenReturn(boxes)

        // When
        val result = mvcTester.get().uri("/api/app/boxes")
            .param("sido", "서울특별시")

        // Then
        result.assertThat()
            .hasStatus(HttpStatus.OK)
            .bodyJson()
            .extractingPath("$.boxes")
            .asArray()
            .hasSize(1)
    }

    @Test
    fun `getBoxes returns 400 when sido is missing`() {
        val result = mvcTester.get().uri("/api/app/boxes")

        result.assertThat()
            .hasStatus(HttpStatus.BAD_REQUEST)
    }
}
```

### @SecuredWebMvcTest 예시 (인증 필요)

```kotlin
@SecuredWebMvcTest(MemberController::class)
class MemberControllerWebMvcTest {

    @Autowired
    lateinit var mvcTester: MockMvcTester

    @MockitoBean
    lateinit var myBoxFacade: MyBoxFacade

    @Test
    fun `getMyBoxes returns member's boxes`() {
        // Given - memberId는 항상 1L 고정
        whenever(myBoxFacade.getMyBoxes(1L)).thenReturn(boxes)

        // When
        val result = mvcTester.get().uri("/api/app/members/me/boxes")

        // Then
        result.assertThat().hasStatus(HttpStatus.OK)
    }
}
```

### 주의사항

- `@WebMvcTest`: `TokenQuery` mock 필수 (JwtAuthenticationFilter 의존성)
- `@SecuredWebMvcTest`: memberId 항상 1L 고정

---

## Fake 구현체 (외부 시스템 대체)

**위치**: `test/kotlin/wodly/_support/fake/`
**명명**: `Fake{Interface}.kt`
**어노테이션**: `@Component`, `@Profile("test")`

외부 시스템(FCM, 이메일, 결제 등)의 테스트용 대체 구현체.

### 규칙

- 실제 구현체에는 `@Profile("!test")` 추가
- 테스트 클래스에 `@ActiveProfiles("test")` 추가
- Fake에서 발송/호출 내역을 기록하여 검증 가능하게

### 예시

```kotlin
// src/test/kotlin/wodly/_support/fake/FakePushSender.kt
@Component
@Profile("test")
class FakePushSender : PushSender {
    private val log = LoggerFactory.getLogger(javaClass)
    val sentMessages = mutableListOf<SentMessage>()

    override fun sendMultiple(tokens: List<String>, title: String, body: String) {
        log.info("[FAKE] 푸시 대량 발송 - tokenCount: {}, title: {}", tokens.size, title)
        sentMessages.add(SentMessage(tokens, title, body))
    }

    fun clear() = sentMessages.clear()

    data class SentMessage(val tokens: List<String>, val title: String, val body: String)
}

// 실제 구현체
@Component
@Profile("!test")
class FcmPushSender : PushSender { ... }

// 테스트
@SpringBootTest
@Transactional
@ActiveProfiles("test")
class SendNotificationFacadeTest(
    private val fakePushSender: FakePushSender
) : FunSpec({
    beforeEach { fakePushSender.clear() }

    test("푸시 발송") {
        // When
        sendNotificationFacade.send(...)

        // Then
        fakePushSender.sentMessages.size shouldBe 1
    }
})
```
