# 최종 검증 체크리스트

---

## 코드 품질

---

```bash
npm run type-check   # 타입 에러 없음
npm run lint         # ESLint 에러 없음
npm test             # 테스트 통과
```

## 기능 검증

---

- [ ] 정상 플로우 동작
- [ ] 로딩 상태 표시
- [ ] 에러 상태 처리 (Toast 또는 에러 UI)
- [ ] 빈 상태 UI

## UI 검증

---

- [ ] 다크모드 정상 표시
- [ ] SafeArea 적용 확인
- [ ] 터치 피드백 동작

## 문서화

---

- [ ] `src/app/{route}/CLAUDE.md` 작성/업데이트
- [ ] `src/components/{domain}/CLAUDE.md` 작성/업데이트
- [ ] `src/services/private/{domain}/CLAUDE.md` 작성/업데이트
