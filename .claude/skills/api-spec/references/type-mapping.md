# 타입 매핑

---

| OpenAPI | Kotlin |
|---------|--------|
| `integer` + `format: int64` | `Long` |
| `integer` | `Int` |
| `string` + `format: date` | `LocalDate` |
| `string` | `String` |
| `array` | `List<T>` |
