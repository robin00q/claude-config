# API 서비스 CLAUDE.md 작성 가이드

---

**경로**: `src/services/private/{domain}/CLAUDE.md`

## 템플릿

---

```markdown
# {도메인} API

---

{도메인} 관련 API 서비스

## 함수 목록

---

| 함수 | 메서드 | 엔드포인트 | 설명 |
|------|--------|-----------|------|
| `getSomething` | GET | `/api/app/...` | 조회 |
| `createSomething` | POST | `/api/app/...` | 생성 |

## 타입

---

```typescript
import type { components } from '@/types/generated/{domain}'

type ResponseType = components['schemas']['SomeResponse']
type RequestType = components['schemas']['SomeRequest']
```

## 사용 예시

---

```typescript
import { getSomething, createSomething } from '@/services/private/{domain}'

// 조회
const data = await getSomething()

// 생성
await createSomething({ field: 'value' })
```
```

## 핵심 규칙

---

1. **함수 목록 테이블 필수**
2. **타입 import 경로 명시**
3. **사용 예시 포함**
4. **에러 케이스 있으면 추가**
