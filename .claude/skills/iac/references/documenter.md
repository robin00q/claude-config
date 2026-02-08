# IaC 문서화 가이드

---

Terraform 코드 변경 시 CLAUDE.md 문서 생성/업데이트 가이드

## 핵심 원칙

---

- **기능 중심**: "어떤 인프라를 관리하는가"에 집중
- **간결함**: 핵심 정보만, 불필요한 수식어 제거
- **맥락 보존**: 이해에 필요한 정보는 생략 금지

## 포함 대상

---

- 인프라 개요 (1-2줄)
- 리소스 목록 (이름, 용도, 스펙)
- 월 비용 요약
- 주요 변수/출력값
- 접속 정보 (IP, endpoint 등)

## 제거 대상

---

- Terraform 코드 전체 복사
- 변수의 default 값 상세
- provider 설정 상세

## 문서 템플릿

---

```markdown
# {디렉토리명}

---

{인프라 개요 1-2줄}

## 리소스

---

| 리소스 | 이름 | 스펙 | 월 비용 |
|--------|------|------|---------|
| Lightsail Instance | wodly-api | micro_3_0 (2 vCPU, 1GB RAM, 40GB SSD), Ubuntu 22.04 | $7 |

## 주요 변수

---

- `db_username`: DB 사용자명
- `db_password`: DB 비밀번호 (sensitive)

## 출력값

---

- `api_public_ip`: API 서버 공인 IP
- `db_endpoint`: DB 접속 엔드포인트
```

## 변환 예시

---

### ❌ 장황한 문서

```markdown
## Lightsail Instance 리소스

이 리소스는 AWS Lightsail에 Ubuntu 22.04 인스턴스를 생성합니다.
bundle_id는 "micro_3_0"을 사용하며...

resource "aws_lightsail_instance" "api" {
  name              = "wodly-api"
  ...
}
```

### ✅ 간결한 문서

```markdown
## 리소스

---

| 리소스 | 이름 | 스펙 | 월 비용 |
|--------|------|------|---------|
| Lightsail Instance | wodly-api | micro_3_0 (2 vCPU, 1GB RAM, 40GB SSD), Ubuntu 22.04 | $7 |
```
