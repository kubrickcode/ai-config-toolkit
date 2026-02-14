# 에이전트 패턴 레퍼런스

에이전트 생성을 위한 상세 패턴 및 고급 설정.

## 프론트매터 고급 필드

### 권한 모드

| 모드                | 동작                    | 사용 사례                |
| ------------------- | ----------------------- | ------------------------ |
| `default`           | 표준 권한 프롬프트      | 일반 대화형 사용         |
| `acceptEdits`       | 파일 편집 자동 승인     | 신뢰할 수 있는 코드 생성 |
| `dontAsk`           | 권한 프롬프트 자동 거부 | 읽기 전용 자동화         |
| `plan`              | 읽기 전용 탐색 모드     | 리서치 및 계획           |
| `bypassPermissions` | 모든 권한 검사 건너뛰기 | 샌드박스 환경 전용       |

### 스킬 프리로딩

시작 시 에이전트 컨텍스트에 스킬 콘텐츠 주입:

```yaml
skills:
  - skill-name-1
  - skill-name-2
```

에이전트가 스킬로 존재하는 특정 도메인 지식을 지속적으로 필요로 할 때 사용.

### 메모리 설정

세션 간 영구 메모리 활성화:

```yaml
memory: project
```

- `user`: `~/.claude/agent-memory/<name>/` — 개인, 모든 프로젝트
- `project`: `.claude/agent-memory/<name>/` — 프로젝트 범위, VCS로 공유
- `local`: `.claude/agent-memory-local/<name>/` — 프로젝트 범위, gitignore

에이전트가 자동으로 메모리 디렉토리에 Read/Write/Edit 권한 획득. `MEMORY.md`의 처음 200줄이 시스템 프롬프트에 주입.

### 훅

에이전트 범위의 라이프사이클 훅:

```yaml
hooks:
  PreToolUse:
    - matcher: Bash
      command: "scripts/validate_command.sh"
  Stop:
    - command: "scripts/cleanup.sh"
```

이벤트: `PreToolUse`, `PostToolUse`, `Stop`

## 본문 구조 패턴

### 패턴 A: 순차 프로세스

절차적 도메인에 최적 (디버깅, 배포, 마이그레이션).

```markdown
You are an expert [ROLE].

When invoked:

1. [분석 단계]
2. [진단 단계]
3. [구현 단계]
4. [검증 단계]

[도메인별 가이던스]

For each task, provide:

- [산출물 1]
- [산출물 2]

Focus on [핵심 원칙].
```

### 패턴 B: 책임 프레임워크

아키텍처/설계 도메인에 최적 (데이터베이스, 백엔드, 프론트엔드).

```markdown
You are a [ROLE] with expertise in [DOMAIN].

## Core Responsibilities

### [영역 1]

- [구체적 가이던스]

### [영역 2]

- [패턴 및 접근법]

## Architecture Patterns

[의사결정 테이블, 프레임워크 비교]

## Implementation Workflow

1. [페이즈 1]
2. [페이즈 2]

## Tool Selection

- **[도구]**: [목적과 근거]

## Key Principles

- [원칙 1]
- [원칙 2]
```

### 패턴 C: 의사결정 프레임워크

평가/분석 도메인에 최적 (기술 어드바이저, 프로덕트 전략가).

```markdown
You are a [ROLE] specializing in [DOMAIN].

## Analysis Process

1. [범위 탐지]
2. [데이터 수집]
3. [프레임워크 적용]
4. [종합]

## Decision Matrix

| 기준 | 가중치 | [옵션 A] | [옵션 B] |
| ---- | ------ | -------- | -------- |

## Output Format

### [보고서 구조]

## Key Principles

- [평가 기준]
```

### 패턴 D: 리서치 + 종합

정보 수집 도메인에 최적 (리서치, 경쟁 분석).

```markdown
You are a [ROLE] expert at [CAPABILITY].

## Focus Areas

- [영역 1]
- [영역 2]

## Research Process

1. [쿼리 설계]
2. [검색 실행]
3. [검증]
4. [종합]

## Output Format

### Key Findings

- [출처 포함 발견사항]

### Source Assessment

| 출처 | 신뢰도 | 비고 |

### Recommendations

- [실행 가능한 인사이트]
```

### 패턴 E: 도구 중심 최적화

메타/시스템 최적화 도메인에 최적.

```markdown
You are a [ROLE] specializing in [SYSTEM].

## Core Workflow

1. [분석]
2. [이슈 식별]
3. [옵션 제시]
4. [구현]
5. [검증]

## [도메인] Expertise

[구체적 지식 영역]

## Diagnostic Process

[이슈 분류 및 측정]

## Optimization Strategies

[구체적 개선 기법]

## Critical Requirements

- [필수 항목]

## Common Pitfalls

- [피해야 할 것]
```

## 품질 체크리스트

모든 에이전트 검증 항목:

- [ ] `description`이 200자 미만이며 "Use PROACTIVELY" 트리거 포함
- [ ] `tools`가 명시적으로 나열됨 (상속에 의존하지 않음)
- [ ] 첫 줄이 전문성 설정: `You are a [ROLE]...`
- [ ] 번호 매긴 워크플로우 단계 포함
- [ ] "Output Format" 섹션 포함 (구조화된 출력 생성 시)
- [ ] "Key Principles" 또는 동등한 마무리 섹션 포함
- [ ] H2/H3 계층만 사용 (H4 없음)
- [ ] Claude가 이미 아는 중복 콘텐츠 없음
- [ ] 마무리가 원칙 또는 성공 기준
- [ ] 파일 크기가 도메인 복잡도에 맞음 (30-300줄)

## 도구 선택 레퍼런스

에이전트 유형별 일반적 도구 조합:

| 에이전트 유형 | 권장 도구                                                      |
| ------------- | -------------------------------------------------------------- |
| 코드 리뷰어   | Read, Grep, Bash                                               |
| 구현          | Read, Write, Edit, Bash, Glob, Grep                            |
| 리서치        | Read, WebFetch, WebSearch                                      |
| 아키텍처      | Read, Write, Edit, Bash, Glob, Grep                            |
| 문서화        | Read, Write, Edit, Glob, Grep                                  |
| 메타/최적화   | Read, Write, Edit, Bash, Glob, Grep, WebFetch, AskUserQuestion |
| 컨텍스트/계획 | Read, Write, Edit, TodoWrite                                   |

**협업 위임 패턴:**

```markdown
## Tool Selection

Essential tools:

- **Read/Grep/Glob**: 코드베이스 분석
- **Edit/Write**: 구현
- **Bash**: 빌드 검증

Collaboration:

- **prompt-engineer**: 복잡한 프롬프트 최적화
- **security-auditor**: 보안 리뷰
- **test-expert**: 테스트 전략 설계
```

## 안티패턴

- **범위 초과**: 에이전트가 도메인의 모든 것을 처리하려 함
- **도구 과잉 부여**: `tools` 필드 생략 (모든 도구 상속)
- **모호한 트리거**: description이 구체적 없이 "X를 도와줍니다"
- **중복 콘텐츠**: Claude가 이미 아는 것을 설명
- **출력 형식 누락**: 에이전트가 일관성 없는 결과 생성
- **잘못된 모델**: 단순 검색에 opus 사용
- **워크플로우 부재**: 본문이 번호 단계 없는 텍스트 벽
