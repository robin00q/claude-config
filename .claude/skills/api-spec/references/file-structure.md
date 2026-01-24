# 파일 구조

---

```
specs/
├── security/security.yaml      # JWT 인증 정의
├── common/openapi.yaml         # 공용 스키마
└── {domain}/openapi.yaml       # 도메인별 스펙
```

## 디렉토리 네이밍

---

**디렉토리명 = URL 경로** (복수형)

| URL 경로 | 디렉토리 |
|----------|----------|
| `/api/app/boxes` | `specs/boxes/` |
| `/api/app/members` | `specs/members/` |
