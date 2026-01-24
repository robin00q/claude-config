## 프로젝트 구조

---

| 프로젝트 | 경로 | 설명 |
|---------|------|------|
| Backend API | `./{project}-api/` | Spring Boot 백엔드 |
| Mobile App | `./{project}-app/` | React Native 앱 |
| API Spec | `./{project}-api-spec/` | OpenAPI 스펙 (서브모듈) |

## Skills 구조

---

```
.claude/skills/
├── commit/       # 커밋 도우미
├── api/          # Spring Boot API 개발
├── api-spec/     # OpenAPI 스펙 작업
└── app/          # React Native 기능 개발
```

## Skills 사용 시 참고

---

| Skill | 작업 디렉토리 | 용도 |
|-------|-------------|------|
| `/api` | `*-api/` | Spring Boot 백엔드 개발 |
| `/app` | `*-app/` | React Native 앱 개발 |
| `/api-spec` | `*-api-spec/` | OpenAPI 스펙 작성 |
| `/commit` | 전체 | 변경사항 커밋 (멀티 디렉토리 지원) |
