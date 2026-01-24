# API 서비스 구현 가이드

---

**경로**: `src/services/private/{domain}/`

## 디렉토리 구조

---

```
services/
├── private/           # 인증 필요 API
│   └── {domain}/
│       ├── index.ts   # API 함수들
│       └── CLAUDE.md
└── public/            # 인증 불필요 API
```

## API 함수 패턴

---

```typescript
import { apiClient } from '@/services/api-client'
import type { components } from '@/types/generated/{domain}'

type ResponseType = components['schemas']['SomeResponse']

export async function getSomething(): Promise<ResponseType> {
  const response = await apiClient.get<ResponseType>('/api/app/something')
  return response.data
}

export async function createSomething(data: CreateRequest): Promise<void> {
  await apiClient.post('/api/app/something', data)
}
```

## 핵심 규칙

---

1. **타입은 generated에서 import** (직접 정의 금지)
2. **apiClient 사용** (fetch 직접 사용 금지)
3. **에러 핸들링은 호출부에서** (서비스는 throw만)
4. **CLAUDE.md 작성 필수**
