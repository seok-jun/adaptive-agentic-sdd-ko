# Workflow

## 0. 저장소 bootstrap contract

Workflow를 도입하기 전에 실제 저장소의 build/test 경로, 모듈 경계, 작업 항목 시스템, 기존 개발 규칙을 식별합니다.

다른 프로젝트의 큰 정책 묶음을 복사하는 것부터 시작하지 않습니다. 작은 루트 agent contract를 만들고 저장소가 실제로 필요로 할 때만 로컬 safeguard를 추가합니다.

`docs/bootstrap.md`를 참고하세요.

## 1. 작업 항목 정의

다음을 정의합니다.

- Priority,
- SDD grade,
- 사용자·비즈니스 영향,
- 목표,
- 범위와 제외 범위,
- 경로 경계가 중요한 경우 수정 허용·금지 path,
- 인수 조건,
- 의존성·blocker,
- 권위 있는 요구사항·설계 참조.

작업 항목은 구현이 시작되기 전에 문제 범위를 좁힙니다. GitHub Issue, Jira, Linear 또는 다른 tracker가 이 역할을 할 수 있습니다.

## 2. 사전 점검(Preflight)

코드 수정 전에:

- 필수 작업 항목 필드가 있는지 확인하고,
- blocker가 해소되었는지 확인하고,
- 요청된 작업을 시작해도 되는지 확인하고,
- 범위 경계를 사용할 수 있는지 확인하고,
- 에이전트가 현재 작업 항목 revision 또는 snapshot에 접근할 수 있는지 확인하고,
- process depth를 선택하기 전에 grade를 확정합니다.

필수 경계나 의존성을 안전하게 판단할 수 없으면 fail closed합니다.

## 3. 필요한 경우 작업 선점 / 격리

병렬 agent 작업에서는:

- lane·작업 항목을 claim하고,
- active 작업과 겹치는지 확인하고,
- 지원된다면 격리된 branch/worktree/sandbox를 사용하고,
- 제품 변경을 허용된 경계 안에 유지합니다.

병렬 격리는 운영 확장 기능이며 모든 저장소의 필수 요구사항은 아닙니다.

## 4. 제한된 AS-IS 분석

현재 메인라인 코드에서 시작합니다.

다음 순서를 선호합니다.

1. 직접 대상 경로,
2. 대상 심볼,
3. 직접 공개 contract,
4. 직접 caller/callee,
5. 관련 지속 문서,
6. 구체적인 증거가 요구할 때만 더 넓은 탐색.

관찰된 동작, 관련 증거, 범위, unknown, 의미 있는 drift를 기록합니다.

Trivial 변경은 일반적으로 별도 AS-IS artifact를 만들지 않습니다. Small 변경은 AS-IS를 작업 항목 또는 구현 note 안에 둘 수 있습니다.

## 5. TO-BE 설계 + 검증 전략

다음을 정의합니다.

- 원하는 동작,
- 유지되어야 하는 동작,
- 실패·오류 동작,
- 필요한 경우 state transition,
- 구체적인 변경 계획,
- 인수 조건과 증거의 매핑.

템플릿을 채우기 위해 선택지를 새로 만들지 않습니다. 실제 경쟁 선택지가 이미 있을 때만 trade-off를 기록합니다.

## 6. Grade gate

위험 등급에 필요한 최소 절차를 적용합니다.

- **Trivial**: 대상 변경 + 대상 검증 + 자체 검토.
- **Small**: 간결한 분석·구현 + 대상 검증 + 자체 검토.
- **Medium**: 명시적 AS-IS/TO-BE/change plan과 contract risk가 정당화할 때 제한된 검토.
- **Large**: 명시적 AS-IS와 plan, 독립 설계/contract review, 로컬 정책상 Human이 소유하는 결정이면 Human 승인, 병합 전 독립 code review.
- **Epic**: 먼저 분할하고 integration ownership을 정의하며 고위험 child/integration lane에 Large 수준 절차를 적용.

## 7. 리비전 식별 가능한 설계 검토

설계 artifact가 독립 검토나 Human 승인을 필요로 하고 협업 환경이 지원한다면 candidate는 immutable 또는 모호하지 않은 revision으로 식별 가능해야 합니다. 예: commit SHA, versioned document revision.

Review Packet은 phase-aware하며 다음 범위로 제한합니다.

- 작업 항목 / 인수 조건,
- 대상 phase artifact,
- 승인된 upstream decision,
- 직접 공개 contract와 invariant,
- 수정 허용·금지 경계,
- 관련 검증 증거.

Review PASS는 Human 승인이 아닙니다.

검토된 artifact가 decision, scope, contract, acceptance semantics에 영향을 주는 방식으로 바뀌면 새 revision에 대해 검토·승인을 반복해야 합니다.

`docs/review-gates.md`를 참고하세요.

## 8. 필요한 경우 Human 승인

Human 승인은 정책 gate이며 reviewer PASS의 다른 이름이 아닙니다.

승인이 필요하면 다음에 귀속합니다.

- 하나의 phase,
- Human에게 제시한 정확한 artifact/revision,
- 승인하는 decision scope,
- 로컬 audit 필요에 따른 승인 시각·actor.

의미 있는 revision 변경 뒤에 모호하거나 오래된 승인을 조용히 재사용하지 않습니다.

## 9. 구현

- 허용된 파일만 변경하고,
- 관련 없는 refactor·feature를 추가하지 않고,
- 승인된 decision과 contract를 유지하고,
- 유용한 경우 직접 회귀 coverage를 추가하고,
- 구현을 인수 조건에 맞춥니다.

## 10. 검증

대상 중심 검증을 먼저 수행합니다. 변경 surface가 정당화할 때 더 넓은 검증을 수행합니다.

실행하지 않은 검증은 PASS가 아니라 UNVERIFIED로 보고합니다.

## 11. 자체 검토

최종 diff를 다음과 비교합니다.

- 작업 항목 범위,
- 인수 조건,
- 수정 허용·금지 경계,
- 승인된 decision,
- 의도하지 않은 동작 변경.

## 12. 필요한 경우 독립 code review

Reviewer는 제품 전체를 재탐색하기보다 승인된 contract와 증거를 기준으로 구현을 반증해야 합니다.

Large/Epic은 기본적으로 독립 code review가 필요합니다. Medium은 shared contract 또는 실패 비용이 정당화할 때 제한된 독립 검토를 사용할 수 있습니다.

## 13. 지속 문서 + 최종 검증

런타임·제품 동작이 바뀌면 planning prose를 복사하지 말고 **최종 관찰된 코드 동작**을 기준으로 지속 문서를 갱신합니다.

그 다음 필수 최종 검증을 실행합니다.

## 14. PR / 병합 게이트

PR에는 다음을 기록합니다.

- 작업 항목,
- 범위,
- 의미 있는 decision,
- 검증 증거,
- 필요한 경우 검토·승인 증거,
- 미검증 또는 blocked 항목.

## 15. 조건부 실기기·런타임 QA

실기기, browser, integration environment, permission, lifecycle, background, media 또는 다른 runtime evidence가 필요하면 필수 관찰 결과를 수집하고 인수 조건에 따라 판정할 때까지 병합을 차단합니다.

## 16. 병합 / 정리 / 해제

필수 증거가 통과한 뒤:

- 병합하고,
- 작업 항목을 종료·갱신하고,
- 로컬 정책상 disposable인 임시 SDD artifact를 제거하고,
- 격리 workspace를 정리하고,
- lane을 해제합니다.
