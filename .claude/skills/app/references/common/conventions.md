# 코드 컨벤션

---

## 파일 네이밍

---

| 유형 | 패턴 | 예시 |
|------|------|------|
| 컴포넌트 | kebab-case | `notification-item.tsx` |
| 유틸리티 | kebab-case | `date-utils.ts` |
| 상수 | kebab-case | `workout-type.ts` |
| 테스트 | `*.test.ts(x)` | `date-utils.test.ts` |

## 컴포넌트 구조

---

```typescript
// 1. imports
import { View, Pressable } from 'react-native'
import { ThemedText } from '@/components/ui'

// 2. types
interface Props {
  data: SomeType
  onPress?: () => void
}

// 3. component
export function SomeComponent({ data, onPress }: Props) {
  // hooks
  const colors = useThemeColors()

  // handlers
  const handlePress = () => { ... }

  // render
  return (...)
}

// 4. styles
const styles = StyleSheet.create({ ... })
```

## 스타일 규칙

---

- `StyleSheet.create()` 사용 (인라인 스타일 최소화)
- 동적 스타일만 인라인: `style={{ backgroundColor: colors.primary }}`
- 테마 색상은 `useThemeColors()` hook 사용

## 세미콜론

---

**사용하지 않음** (프로젝트 규칙)

```typescript
// ✅ Good
const value = 123
function doSomething() { }

// ❌ Bad
const value = 123;
function doSomething() { };
```

## Export 규칙

---

- Named export 선호 (default export 지양)
- index.ts로 re-export 시 barrel pattern 사용

```typescript
// components/ui/index.ts
export { ThemedText } from './themed-text'
export { Button } from './button'
```

## 아이콘

---

**phosphor-react-native 사용** (IconSymbol 대신)

```typescript
import { CaretDown, CalendarBlank, Cube } from 'phosphor-react-native'

<CaretDown size={16} color={colors.textSecondary} weight="bold" />
```

- 공식 문서: https://phosphoricons.com
- weight 옵션: `thin`, `light`, `regular`, `bold`, `fill`, `duotone`
