# OpenAPI 작성 가이드

---

api-spec/CLAUDE.md 기반 핵심 규칙 정리

## 파일 구조

---

```
specs/
├── security/security.yaml      # JWT 인증 정의
├── common/openapi.yaml         # 공용 스키마
└── {domain}/openapi.yaml       # 도메인별 스펙
```

- **디렉토리명 = URL 경로** (복수형)
  - `/api/app/boxes` → `specs/boxes/`

## yaml 템플릿

---

```yaml
openapi: 3.0.3
info:
  title: WODly API - {Domain}
  version: 1.0.0
  description: {도메인} 관련 API

paths:
  /api/app/{resource}:
    post:
      tags: [{Domain}]
      summary: {요약}
      description: {상세 설명}
      operationId: {operation}
      security:
        - bearerAuth: []  # 인증 필요 시
      requestBody:
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/{Action}Request'
      responses:
        '201':
          description: {응답 설명}

components:
  securitySchemes:
    bearerAuth:
      $ref: '../security/security.yaml'
  schemas:
    {Action}Request:
      type: object
      description: {스키마 설명}
      properties:
        field:
          type: string
          description: {필드 설명}
```

## 네이밍 규칙

---

| 유형 | 패턴 | 예시 |
|------|------|------|
| Request | `{Action}{Domain}Request` | `RegisterBoxRequest` |
| Response | `{Domain}Response` | `BoxResponse` |

## 공용 스키마 참조

---

`specs/common/openapi.yaml` 스키마는 사용처에서 직접 참조:

```yaml
# ✅ 올바른 예
properties:
  gender:
    $ref: '../common/openapi.yaml#/components/schemas/Gender'

# ❌ 잘못된 예 (재정의 금지)
components:
  schemas:
    Gender:
      $ref: '../common/openapi.yaml#/components/schemas/Gender'
```

**공용 스키마 목록**: Gender, RecordType, ScalingOption, WorkoutType, TimeRequest, TimeResponse, RecordRequest, RecordResponse, CursorPageMeta

## 타입 매핑

---

| OpenAPI | Kotlin |
|---------|--------|
| `integer` + `format: int64` | `Long` |
| `integer` | `Int` |
| `string` + `format: date` | `LocalDate` |
| `string` | `String` |
| `array` | `List<T>` |

## description 필수

---

모든 요소에 description 작성:
- paths.{method}.summary/description
- responses.{code}.description
- schemas.{name}.description
- properties.{name}.description
