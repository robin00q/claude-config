# 코드리뷰 가이드 (API)

---

## 실행 방법

---

1. `git diff main...HEAD --name-only` 로 변경 파일 목록 확보
2. Task 도구로 코드리뷰 에이전트 실행:

```
Task(
  subagent_type = "general-purpose",
  prompt = """
  아래 파일들을 읽고 코드리뷰를 수행해주세요.
  리뷰 결과만 출력하고, 코드를 수정하지 마세요.

  ## 변경 파일
  {변경 파일 목록}

  ## 리뷰 기준

  ### 아키텍처 (Hexagonal)
  - Domain → Application → Adapter 계층 의존성 방향 준수
  - Service: Repository만 의존 / Facade: Query/Mutation 인터페이스만 의존
  - Domain에 프레임워크 의존성 없음

  ### 예외/로깅
  - 예외 throw 전 로깅 (ERROR: 비정상, WARNING: 보안, INFO: 사용자 행동)
  - 예외 메시지에 ID 포함 금지, 로그에만 포함
  - BusinessException 상속 여부

  ### 테스트
  - 정상/예외/경계값 케이스 커버리지
  - Domain: 단위 테스트 / Service: @SpringBootTest / Controller: @WebMvcTest

  ### 코드 품질
  - 불필요한 코드, 중복 로직
  - 네이밍 컨벤션 (Kotlin 표준)
  - 보안 취약점 (SQL injection, 인증/인가 누락 등)

  ## 출력 형식

  ### 요약
  전체적인 코드 품질 평가 (1-2줄)

  ### 이슈 (있는 경우만)
  | 파일 | 라인 | 심각도 | 설명 |
  |-----|------|--------|------|
  | ... | ... | Critical/Warning/Info | ... |

  ### 개선 제안 (선택)
  - 필수는 아니지만 개선하면 좋을 사항
  """
)
```

3. 리뷰 결과를 사용자에게 전달
