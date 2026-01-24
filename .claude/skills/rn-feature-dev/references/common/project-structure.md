# 프로젝트 구조

---

```
src/
├── app/                  # 화면 (expo-router, 파일 기반 라우팅)
│   ├── (auth)/           # 인증 화면 그룹
│   └── (tabs)/           # 메인 탭 화면 그룹
│
├── types/generated/      # API 타입 (자동 생성, 수정 금지)
│
├── models/               # 앱 도메인 모델
│
├── services/             # API 서비스
│   └── private/          # 인증 필요 API
│
├── contexts/             # 전역 상태 (React Context)
│
├── components/           # 재사용 컴포넌트
│   ├── ui/               # 공통 UI (Button, Input 등)
│   └── {domain}/         # 도메인별 컴포넌트
│
├── hooks/                # Custom hooks
│
├── constants/            # 상수, 테마, 설정
│
└── utils/                # 유틸리티 함수
```

## 주요 경로 규칙

---

| 용도 | 경로 |
|------|------|
| 화면 | `src/app/{route}/index.tsx` |
| API 서비스 | `src/services/private/{domain}/` |
| 컴포넌트 | `src/components/{domain}/` |
| 상수 | `src/constants/{domain}.ts` |
| 테스트 | `src/{domain}/__tests__/` |

## import 별칭

---

```typescript
import { Something } from '@/components/ui'  // src/components/ui
import { api } from '@/services/private/wod' // src/services/private/wod
```
