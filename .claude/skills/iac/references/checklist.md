# IaC 완료 체크리스트

---

## Phase 1: 설계

---

- [ ] 필요한 리소스 목록 정리
- [ ] 리소스 간 의존성 확인
- [ ] 변수/출력값 정의

## Phase 2: 비용 안내

---

- [ ] 리소스별 변경 내역 + 비용 영향 표로 안내
- [ ] 사용자 확인 완료

## Phase 3: 구현

---

- [ ] main.tf 작성/수정
- [ ] variables.tf 작성/수정
- [ ] outputs.tf 작성/수정
- [ ] terraform.tfvars 민감 정보 분리
- [ ] .gitignore에 tfvars, .terraform/ 포함

## Phase 4: 검증

---

- [ ] `terraform fmt` 통과
- [ ] `terraform validate` 통과
- [ ] `terraform plan` 확인

## Phase 5: 문서화

---

- [ ] 관련 CLAUDE.md 업데이트
- [ ] 새 디렉토리 시 CLAUDE.md 생성

## Phase 6: 적용 안내

---

- [ ] 사용자에게 apply 명령어 안내
- [ ] 주의사항 (다운타임, 데이터 손실 등) 안내
