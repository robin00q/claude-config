# YAML 템플릿

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

## description 작성

---

- 모든 요소에 description 필수 (paths, responses, schemas, properties)
- 특수문자(`?`, `:`, `#` 등) 포함 시 `""`로 감싸기

```yaml
# 올바른 예
description: "사용자 정보가 없습니다. 다시 시도해주세요."

# 잘못된 예
description: 사용자 정보가 없습니다. 다시 시도해주세요.
```
