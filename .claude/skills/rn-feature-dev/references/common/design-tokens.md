# 디자인 토큰

---

**경로**: `src/constants/design-tokens.ts`, `src/constants/theme.ts`

## Spacing (여백)

---

```typescript
import { Spacing } from '@/constants/design-tokens'

Spacing.xs   // 4dp
Spacing.sm   // 8dp
Spacing.md   // 12dp
Spacing.lg   // 16dp
Spacing.xl   // 20dp
Spacing.xxl  // 24dp
Spacing.xxxl // 32dp
```

## BorderRadius (모서리)

---

```typescript
BorderRadius.sm   // 8
BorderRadius.md   // 12
BorderRadius.lg   // 16
BorderRadius.full // 9999
```

## FontSize (글자 크기)

---

```typescript
FontSize.xs    // 12
FontSize.sm    // 14
FontSize.md    // 16 (기본)
FontSize.lg    // 18
FontSize.xl    // 20
FontSize.xxl   // 24
```

## Colors (테마 색상)

---

```typescript
import { Colors } from '@/constants/theme'
import { useColorScheme } from '@/hooks/use-color-scheme'

const colorScheme = useColorScheme()
const colors = Colors[colorScheme ?? 'light']

colors.primary         // 토스 블루 (#3182F6)
colors.text            // 주요 텍스트
colors.textSecondary   // 보조 텍스트
colors.background      // 배경
colors.cardBackground  // 카드 배경
colors.border          // 테두리
colors.error           // 에러
```

## 사용 원칙

---

❌ 하드코딩 금지
```typescript
padding: 16
fontSize: 18
color: '#3182F6'
```

✅ 토큰 사용
```typescript
padding: Spacing.lg
fontSize: FontSize.lg
color: colors.primary
```
