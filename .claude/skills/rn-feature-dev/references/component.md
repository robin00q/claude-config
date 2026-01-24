# 컴포넌트 분리 가이드

---

**경로**: `src/components/{domain}/`

## 분리 기준

---

| 조건 | 위치 |
|------|------|
| 재사용 가능 | `src/components/{domain}/` |
| 화면 전용 + 복잡 | `src/components/{domain}/` |
| 화면 전용 + 단순 | 화면 파일 내 유지 |

## 컴포넌트 템플릿

---

```typescript
import { StyleSheet, View, Pressable } from 'react-native'
import { ThemedText } from '@/components/themed-text'
import { Colors } from '@/constants/theme'
import { useColorScheme } from '@/hooks/use-color-scheme'
import { Spacing, BorderRadius } from '@/constants/design-tokens'

interface SomeItemProps {
  data: DataType
  onPress?: (data: DataType) => void
}

export function SomeItem({ data, onPress }: SomeItemProps) {
  const colorScheme = useColorScheme() ?? 'light'
  const colors = Colors[colorScheme]

  return (
    <Pressable
      onPress={() => onPress?.(data)}
      style={({ pressed }) => [
        styles.container,
        { backgroundColor: colors.cardBackground, opacity: pressed ? 0.7 : 1 },
      ]}
    >
      {/* 콘텐츠 */}
    </Pressable>
  )
}

const styles = StyleSheet.create({
  container: {
    borderRadius: BorderRadius.md,
    padding: Spacing.lg,
  },
})
```

## 핵심 규칙

---

1. **Props 인터페이스 명시**
2. **useColorScheme으로 테마 대응**
3. **design-tokens 사용** (하드코딩 금지)
4. **CLAUDE.md 작성 필수**
