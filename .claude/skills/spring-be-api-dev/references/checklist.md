# 개발 완료 체크리스트

---

## Phase 1: Domain

- [ ] Entity 작성 (`BaseEntity` 상속, 팩토리 메서드)
- [ ] Value Object 작성
- [ ] Command DTO 작성 (`dto/` 하위)
- [ ] Exception 작성 (`BusinessException` 상속)

## Phase 2: Service

- [ ] Mutation 인터페이스 작성
- [ ] Query 인터페이스 작성
- [ ] Repository 인터페이스 작성
- [ ] Service 구현체 작성

## Phase 3: Facade (필요시)

- [ ] Facade 작성 (여러 Aggregate 조율 시만)
- [ ] Input/Output 정의

## Phase 4: API Spec (Controller 작성 시)

- [ ] api-spec 서브모듈에서 OpenAPI yaml 작성
- [ ] 서브모듈 커밋 & 푸시
- [ ] 현재 레포 서브모듈 최신화

## Phase 5: Controller (필요시)

- [ ] Controller 작성
- [ ] Request → Command 변환 로직

## Phase 6: Fixture

- [ ] Entity Fixtures 작성
- [ ] Command Fixtures 작성

## Phase 7: 테스트

- [ ] Domain 테스트 작성
- [ ] Service 테스트 작성
- [ ] Facade 테스트 작성 (해당 시)
- [ ] Controller 테스트 작성 (해당 시)
- [ ] 모든 테스트 통과: `./gradlew test`

## Phase 8: 문서화

- [ ] domain/{aggregate}/CLAUDE.md 업데이트
- [ ] application/service/{aggregate}/CLAUDE.md 업데이트
- [ ] application/facade/{domain}/CLAUDE.md 업데이트 (해당 시)

## 최종 검증

- [ ] 빌드 성공: `./gradlew build`
- [ ] 린트 통과: `./gradlew ktlintCheck`
