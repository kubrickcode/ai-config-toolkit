# API 규칙

## 공통

### 필드 네이밍

- Boolean: `is/has/can` 접두사
- Date: `~At` 접미사 (ISO 8601 UTC)
- 일관된 용어 사용 ("create" 또는 "add" 중 하나로 통일 등)

### 페이지네이션 (커서 기반)

- REST: `?cursor=xyz&limit=20` → `{ data, nextCursor, hasNext }`
- GraphQL: Relay Connection (`first`, `after`, `PageInfo`)

### 정렬

- 파라미터: `sortBy`, `sortOrder` (REST) 또는 `orderBy` 배열 (GraphQL)
- 복수 기준 지원
- 기본값 명시

### 필터링

- 범위: `{ min, max }` 또는 `{ gte, lte }`
- 복잡한 조건: 중첩 객체

## REST

- 중첩 리소스: 최대 2단계
- 동사는 리소스로 표현 불가할 때만 (`/users/:id/activate`)
- 목록 응답: `data` + 페이지네이션 정보
- 생성: 201 + 리소스 (민감 정보 제외)
- 에러: RFC 7807 ProblemDetail (`type`, `title`, `status`, `detail`, `instance`)
- 배치: `/batch` 접미사 + 성공/실패 카운트

## GraphQL

### 타입 네이밍

- Input: `{Verb}{Type}Input`
- Connection: `{Type}Connection`
- Edge: `{Type}Edge`

### Input 설계

- 생성/수정 분리 (필수 vs 선택 필드)
- 중첩 피하기 - ID만 사용

### 에러 처리

- 기본: `errors[].extensions`에 `code`, `field`
- 타입 안전: Union 타입 (`User | ValidationError`)

### 성능

- N+1: DataLoader 필수

### 문서화

- 모든 타입에 `"""description"""` 필수
- Input 제약조건 명시
- Deprecation: `@deprecated(reason: "...")`, 타입 삭제 금지
