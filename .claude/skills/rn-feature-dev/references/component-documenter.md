# 컴포넌트 CLAUDE.md 작성 가이드

---

**경로**: `src/components/{domain}/CLAUDE.md`

## 템플릿

---

```markdown
# {도메인} 컴포넌트

---

{간단한 설명}

## {ComponentName}

---

{컴포넌트 설명}

### UI

```
┌─────────────────────────────────────┐
│ [아이콘] 타입명              시간   │
│ 내용                                │
└─────────────────────────────────────┘
```

### Props

```typescript
interface {ComponentName}Props {
  data: DataType
  onPress?: (data: DataType) => void
}
```

### 스타일

- 배경: `cardBackground`
- 카드: `BorderRadius.md`, `Spacing.lg` 패딩
- 상태별 스타일 차이

## API

---

- `GET /api/app/...` - 설명 (컴포넌트와 관련된 API)
```

## 핵심 규칙

---

1. **UI 다이어그램 필수** (상태별로 여러 개 가능)
2. **Props 인터페이스 명시**
3. **스타일 요약** (design-tokens 기준)
4. **관련 API 있으면 포함**
