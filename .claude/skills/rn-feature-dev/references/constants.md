# 상수 정의 가이드

---

**경로**: `src/constants/{domain}.ts`

## 작성 대상

---

- 도메인별 라벨 매핑 (`{DOMAIN}_LABELS`)
- 타입별 설정 객체 (`{DOMAIN}_CONFIG`)
- 아이콘/색상 매핑

## 예시

---

```typescript
// notification-type.ts
import { Icon, Cube, Bell } from 'phosphor-react-native'

type NotificationType = 'BOX_WOD_REGISTERED' | 'GENERAL'

export const NOTIFICATION_TYPE_CONFIG: Record<NotificationType, { icon: Icon; label: string }> = {
  BOX_WOD_REGISTERED: { icon: Cube, label: 'WOD 등록' },
  GENERAL: { icon: Bell, label: '알림' },
}
```

## 핵심 규칙

---

1. **하드코딩 금지** - 화면에서 직접 문자열/아이콘 사용 X
2. **한 곳에서 관리** - 동일 도메인 상수는 한 파일에
3. **타입 안전성** - Record<Type, Config> 패턴 사용
