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

## 바텀시트 (Bottom Sheet)

---

**Animated.spring 필수 사용** (Modal의 기본 slide 애니메이션 사용 금지)

```typescript
import { Modal, Animated, Dimensions } from 'react-native'
import { useEffect, useRef } from 'react'
import { useSafeAreaInsets } from 'react-native-safe-area-context'

const SCREEN_HEIGHT = Dimensions.get('window').height

function BottomSheet({ visible, onClose, children }) {
  const insets = useSafeAreaInsets()
  const slideAnim = useRef(new Animated.Value(SCREEN_HEIGHT)).current

  useEffect(() => {
    if (visible) {
      // 열기: 스프링 애니메이션
      Animated.spring(slideAnim, {
        toValue: 0,
        useNativeDriver: true,
        tension: 65,
        friction: 11,
      }).start()
    } else {
      // 닫기: 타이밍 애니메이션
      Animated.timing(slideAnim, {
        toValue: SCREEN_HEIGHT,
        duration: 250,
        useNativeDriver: true,
      }).start()
    }
  }, [visible])

  return (
    <Modal visible={visible} transparent animationType="fade">
      <Animated.View
        style={{
          transform: [{ translateY: slideAnim }],
          paddingBottom: Math.max(insets.bottom, Spacing.xl),
        }}
      >
        {children}
      </Animated.View>
    </Modal>
  )
}
```

**핵심 설정:**
- Modal: `animationType="fade"` (배경만 페이드)
- 시트: `Animated.spring` (tension: 65, friction: 11)
- SafeArea: `useSafeAreaInsets()`로 하단 여백 보장
