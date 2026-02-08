# Terraform 컨벤션

---

## 파일 구조

---

```
wodly-iac/
├── .gitignore
├── CLAUDE.md
├── main.tf              # provider + 리소스 정의
├── variables.tf         # 변수 선언
├── outputs.tf           # 출력값
├── terraform.tfvars     # 실제 값 (.gitignore 대상)
└── scripts/             # 서버 초기화 스크립트 등
```

리소스가 많아지면 기능별 파일 분리:

```
├── lightsail.tf         # Lightsail 리소스
├── network.tf           # 네트워크 관련
└── ...
```

## 네이밍 규칙

---

| 구분 | 형식 | 예시 |
|------|------|------|
| 리소스 이름 | `wodly-{용도}` | `wodly-api`, `wodly-db` |
| 변수명 | snake_case | `db_username`, `instance_bundle_id` |
| 출력명 | snake_case | `api_public_ip`, `db_endpoint` |
| 태그 | `Project = "wodly"` | 모든 리소스에 공통 태그 |

## 변수 선언

---

```hcl
variable "db_username" {
  description = "DB 마스터 사용자명"
  type        = string
}

variable "db_password" {
  description = "DB 마스터 비밀번호"
  type        = string
  sensitive   = true
}
```

- `sensitive = true`: 비밀번호, 시크릿 등
- `default`: 환경에 무관한 값만 기본값 설정
- `description`: 항상 작성

## .gitignore

---

```
*.tfstate
*.tfstate.*
.terraform/
terraform.tfvars
*.tfvars
!*.tfvars.example
crash.log
```

## 태그 컨벤션

---

모든 리소스에 공통 태그:

```hcl
tags = {
  Project = "wodly"
}
```
