# Review Gates

검토 깊이는 위험도와 phase에 따라 달라집니다.

## 검토 원칙

- 검토는 재설계보다 **반증(falsify)**에 집중합니다.
- Reviewer context는 현재 phase와 직접 contract로 제한합니다.
- Review PASS는 증거이지 Human 승인이 아닙니다.
- 검토된 artifact가 revision으로 식별된다면 finding과 PASS는 해당 revision에만 적용됩니다.
- decision·scope·contract semantics가 의미 있게 바뀌면 새 검토가 필요합니다.

## Phase-aware Review Packet

설계 Review Packet에는 일반적으로 다음만 포함합니다.

- 작업 항목 identity와 인수 조건,
- 현재 phase(`AS-IS`, `PLAN`, `CODE`),
- 가능한 경우 정확한 대상 artifact/revision,
- 승인된 upstream decision,
- 직접 caller/callee, 공개 contract, invariant,
- 수정 허용·금지 경계,
- 관련 증거.

명시적 작업이 architecture discovery인 경우가 아니라면 저장소 전체를 reviewer에게 넘겨주고 새 설계를 요구하지 않습니다.

## Small

기본:

- implementation agent 자체 검토.

구체적인 이유가 있을 때 독립 검토를 선택적으로 적용합니다.

## Medium

기본:

- shared contract, ambiguity 또는 integration risk가 정당화할 때 제한된 독립 검토.

Reviewer는 phase packet 범위 안에 머뭅니다.

## Large

기본적으로 다음이 필요합니다.

1. 구현 전 독립 설계/contract review,
2. 병합 전 독립 code review.

설계 검토는 다음을 확인합니다.

- architecture·boundary 위반,
- 만족할 수 없는 인수 조건,
- 안전하지 않은 failure behavior,
- 누락된 integration ownership,
- 해결되지 않은 contract 선택,
- 구현에 의미 있게 영향을 주는 hidden assumption.

코드 검토는 다음을 확인합니다.

- diff와 작업 항목·승인된 plan의 일치,
- 검증 증거,
- regression,
- scope creep,
- 안전하지 않은 구현 세부사항.

## Epic

다음이 필요합니다.

- breakdown·integration ownership 검토,
- 고위험 child/integration work의 설계 검토,
- Large와 유사한 lane의 독립 code review.

## Blind Audit escalation

주요 위험이 누락, hidden assumption, reviewer anchoring일 때 Blind Audit이 유용합니다.

사용할 때:

- author와 primary contract reviewer 모두와 독립된 reviewer/session을 사용하고,
- auditor의 최초 verdict 전에 primary review verdict·finding을 미리 제공하지 않고,
- 동일한 제한된 phase contract와 target revision을 제공하고,
- 최초 blind verdict가 고정된 뒤에만 이전 finding을 reconciliation합니다.

도구가 지원한다는 이유만으로 Blind Audit을 universal하게 만들지 않습니다.

## Finding 분류

Finding은 실제로 무엇을 막는지에 따라 분류합니다.

- **Human Decision blocker** — 경쟁하는 제품·아키텍처 의미에 Human 선택이 필요함.
- **Execution-contract blocker** — 여러 기술적으로 합리적인 구현이 contract·persistence·recovery·scope 의미를 바꿀 수 있어 구현 전에 한 방향으로 고정해야 함.
- **Implementation finding** — 승인된 contract가 명확하며 코드·fixture·기계적 수정이 필요함.
- **Non-blocking observation** — 현재 acceptance·decision 경계 밖의 유용한 개선.

이 분류는 구현 결함 때문에 제품 decision이 불필요하게 다시 열리는 것을 막습니다.

## Human approval provenance

Human 승인이 필요하면 무엇을 승인했는지 알 수 있을 만큼의 provenance를 기록합니다.

- phase,
- target artifact/revision,
- decision scope,
- 로컬 정책에 따른 approver identity,
- 로컬 정책에 따른 승인 시각.

Reviewer PASS, 일반적인 대화 동의, 오래된 artifact revision에서 승인을 추정하지 않습니다.
