# SDD Workflow — Starter Local Contract

이 문서는 starter Skill이 참조하는 로컬 process contract입니다. `AGENTS.md`를 계속 키우는 대신 저장소에 맞게 이 문서를 조정합니다.

## Grades

### Trivial

비동작 변경만 허용합니다. executable/test/build/dependency/schema/contract/workflow semantics를 바꾸지 않습니다.

### Small

하나의 구현 경계, 낮은 실패 비용, 좁은 검증 범위.

### Medium

Large 수준 실패 비용은 아니지만 여러 경계·shared contract·integration work가 관련됨.

### Large

고비용, 되돌리기 어려움, security/privacy, migration, recovery/consistency, authorization, cost/quota 또는 durable public contract 위험.

### Epic

독립적으로 전달 가능한 여러 capability 또는 큰 boundary 변경. 먼저 분할합니다.

## 필수 깊이

| Grade | AS-IS/PLAN | Design review | Human approval | Code review |
| --- | --- | --- | --- | --- |
| Trivial | 별도 artifact 없음 | 없음 | 없음 | self |
| Small | 작업 항목 / note | 선택 | 없음 | self |
| Medium | 명시적 | risk-based | 로컬 정책 | risk-based |
| Large | 명시적 | 필수 | Human-owned일 때 | 필수 |
| Epic | breakdown + 고위험 lane 명시 | 고위험 lane 필수 | Human-owned일 때 | Large-like lane 필수 |

## Approval semantics

Reviewer PASS는 Human 승인을 만들지 않습니다.

Human 승인이 필요하면 다음에 귀속합니다.

- phase,
- artifact/revision,
- decision scope.

의미 있는 변경은 변경된 semantics에 대한 오래된 review/approval을 무효화합니다.

## Verification semantics

PASS / FAIL / BLOCKED / UNVERIFIED를 사용합니다. 실행하지 않은 check를 PASS로 취급하지 않습니다.

## Local extensions

필요하면 다음과 같은 저장소 전용 규칙을 이 문서 또는 전용 conditional docs에 추가합니다.

- module ownership,
- build/test preflight,
- migration,
- device/browser QA,
- shell/encoding rule,
- 조직 전용 review system.
