# 공용 스키마 참조

---

## 위치

---

`specs/common/openapi.yaml`

## 규칙

---

- 2개 이상 도메인에서 사용하는 상수/enum은 `common/`에 정의
- 사용처에서 직접 참조 (재정의 금지)

```yaml
# 올바른 예
properties:
  gender:
    $ref: '../common/openapi.yaml#/components/schemas/Gender'

# 잘못된 예 (재정의 금지)
components:
  schemas:
    Gender:
      $ref: '../common/openapi.yaml#/components/schemas/Gender'
```

## 공용 스키마 목록

---

Gender, RecordType, ScalingOption, WorkoutType, TimeRequest, TimeResponse, RecordRequest, RecordResponse, CursorPageMeta
