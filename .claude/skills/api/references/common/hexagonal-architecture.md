# Hexagonal Architecture

---

```
src/main/kotlin/wodly/
├── domain/
│   ├── BaseEntity.kt
│   ├── exception/BusinessException.kt
│   ├── shared/                    # 공유 Value Objects
│   │   └── *.kt
│   └── {aggregate}/
│       ├── *Entity.kt
│       ├── *ValueObject.kt
│       ├── *Status.kt, *Type.kt   # Enum
│       ├── dto/*Command.kt
│       └── exception/*Exception.kt
│
├── application/
│   ├── service/{aggregate}/
│   │   ├── *Service.kt
│   │   ├── provided/
│   │   │   ├── *Mutation.kt
│   │   │   └── *Query.kt
│   │   └── required/
│   │       ├── *Repository.kt
│   │       └── *Sender.kt          # 외부 서비스 포트
│   └── facade/{domain}/
│       └── *Facade.kt
│
├── adapter/
│   ├── in/webapi/
│   │   ├── common/GlobalExceptionHandler.kt
│   │   └── app/{aggregate}/
│   │       ├── *Controller.kt
│   │       ├── *RequestMapper.kt
│   │       └── *ResponseMapper.kt
│   └── out/
│       └── {external}/*Adapter.kt
│
└── config/
    ├── jpa/JpaConfig.kt
    ├── cache/CacheConfig.kt
    ├── logging/RequestLoggingFilter.kt
    └── security/
        ├── SecurityConfig.kt
        └── Jwt*.kt
```

## 핵심 규칙

---

| 계층 | 역할 | 의존 대상 |
|------|------|----------|
| Service | 단일 Aggregate 처리 | Repository만 |
| Facade | 여러 Aggregate 조율 | Query/Mutation 인터페이스 |
| Controller | HTTP 요청 처리 | Facade 또는 Query/Mutation |

## JPA 의존성

---

- JPA는 외부 의존성으로 간주하지 않음
- Entity는 `domain/`에 직접 위치
- JpaRepository는 `required/` 포트에 위치

## 외부 서비스 Adapter

---

외부 API 호출 시 Port/Adapter 패턴 사용

| 위치 | 역할 |
|------|------|
| `required/{Interface}.kt` | 인터페이스 (Port) |
| `adapter/out/{service}/{Impl}.kt` | 구현체 (Adapter) |
