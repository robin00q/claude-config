# 코드리뷰 가이드 (IaC)

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

  ### 보안
  - SSH(22) 키 인증만 허용
  - DB publicly_accessible = false
  - 민감 정보 sensitive = true 변수 분리
  - 불필요한 포트 개방 없음
  - Security Group 최소 권한 원칙

  ### 비용
  - 리소스 스펙 과도하지 않은지
  - 불필요한 리소스 생성 없는지

  ### Terraform 규칙
  - 네이밍 컨벤션 (snake_case, 프로젝트 접두사)
  - 하드코딩 값 → 변수 분리
  - output 정의 적절성
  - provider 버전 고정

  ### 상태 관리
  - terraform.tfvars, *.tfstate 가 .gitignore에 포함
  - 데이터 손실 가능성 있는 변경 (lifecycle prevent_destroy 등)

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
