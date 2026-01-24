# 테스트 작성 가이드

---

**Jest + jest-expo 사용**

## 테스트 파일 위치

---

```
src/{domain}/
├── some-component.tsx
├── some-utils.ts           # 순수 함수 분리
└── __tests__/
    ├── some-utils.test.ts  # 순수 함수 테스트
    └── some-component.test.tsx  # 컴포넌트 테스트
```

## 순수 함수 테스트

---

**JSX 파일에서 직접 import 시 Jest 파싱 실패 → 순수 함수는 별도 파일로 분리**

```typescript
// some-utils.ts
export function formatValue(value: number): string {
  return value.toFixed(2)
}

// __tests__/some-utils.test.ts
import { formatValue } from '../some-utils'

describe('formatValue', () => {
  it('소수점 2자리로 포맷한다', () => {
    expect(formatValue(123.456)).toBe('123.46')
  })
})
```

## 컴포넌트 테스트

---

```typescript
import { render, fireEvent } from '@testing-library/react-native'
import { SomeComponent } from '../some-component'

// 필수 모킹
jest.mock('@/hooks/use-color-scheme', () => ({
  useColorScheme: () => 'light',
}))

jest.mock('phosphor-react-native', () => ({
  IconName: () => 'IconName',
}))

describe('SomeComponent', () => {
  it('버튼 클릭 시 onPress 호출', () => {
    const onPress = jest.fn()
    const { getByTestId } = render(<SomeComponent onPress={onPress} />)

    fireEvent.press(getByTestId('some-button'))
    expect(onPress).toHaveBeenCalledTimes(1)
  })
})
```

## testID 사용

---

인터랙션 요소에 `testID` 추가

```typescript
// 컴포넌트
<Pressable testID="submit-button" onPress={onSubmit}>

// 테스트
fireEvent.press(getByTestId('submit-button'))
```

## 실행 명령어

---

```bash
npm test                    # 전체 테스트
npm run test:watch          # 변경 감지 모드
npm run test:coverage       # 커버리지 리포트
```
