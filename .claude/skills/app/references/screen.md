# 화면 구현 가이드

---

**경로**: `src/app/{route}/`

## 파일 구조

---

```
{route}/
├── index.tsx      # 메인 화면
├── _layout.tsx    # 레이아웃 (필요시)
├── [id].tsx       # 동적 라우트 (필요시)
└── CLAUDE.md
```

## 화면 템플릿

---

```typescript
import { useState, useEffect } from 'react'
import { ActivityIndicator, FlatList } from 'react-native'
import { ThemedView } from '@/components/themed-view'
import { ThemedText } from '@/components/themed-text'
import { useToast } from '@/contexts/toast'

export default function SomeScreen() {
  const [data, setData] = useState<DataType[]>([])
  const [isLoading, setIsLoading] = useState(true)
  const { showToast } = useToast()

  useEffect(() => {
    loadData()
  }, [])

  const loadData = async () => {
    try {
      const response = await getData()
      setData(response)
    } catch (error) {
      showToast('데이터를 불러오지 못했습니다', 'error')
    } finally {
      setIsLoading(false)
    }
  }

  if (isLoading) {
    return (
      <ThemedView style={styles.center} useSafeArea>
        <ActivityIndicator />
      </ThemedView>
    )
  }

  return (
    <ThemedView style={styles.container} useSafeArea>
      {/* 콘텐츠 */}
    </ThemedView>
  )
}
```

## 체크리스트

---

- [ ] `useSafeArea` 적용 (최상위 ThemedView만)
- [ ] 로딩 상태 처리
- [ ] 에러 상태 처리 (Toast 또는 에러 UI)
- [ ] 빈 상태 UI
- [ ] 다크모드 확인
