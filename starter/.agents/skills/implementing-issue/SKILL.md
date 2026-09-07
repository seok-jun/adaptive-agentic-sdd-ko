---
name: implementing-issue
description: 승인된 저장소 작업 항목을 제한된 분석, 위험 적응형 SDD gate, 범위 내 수정, 검증, 검토를 사용해 구현한다.
---

# Implementing Issue — Starter

## Inputs

필수:

1. 현재 작업 항목 identity와 최신 body/snapshot,
2. `docs/sdd-workflow.md`,
3. target code와 direct contract,
4. 관련 있을 때만 linked architecture/product docs.

일반적인 프로젝트 이해를 위해 관련 없는 domain을 읽지 않습니다.

## Preflight

제품 수정 전에:

1. 목표, 범위·제외 범위, 인수 조건, dependency·blocker를 확인하고,
2. grade를 확정하고,
3. path ownership이 중요한 경우 수정 허용·금지 경계를 확인하고,
4. 현재 phase에 필요한 approval이 있는지 확인하고,
5. 중요한 인수 조건의 verification strategy를 식별합니다.

필수 정보를 안전하게 판단할 수 없으면 수정하지 않고 blocker를 보고합니다.

## Grade gate

- **Trivial**: 별도 SDD artifact 없음. 대상 변경 + 대상 검증 + 자체 검토.
- **Small**: 작업 항목을 plan으로 사용 가능. 제한된 검사 + 대상 검증 + 자체 검토.
- **Medium**: 명시적 AS-IS / TO-BE / change plan. contract/integration risk가 정당화할 때 제한된 검토.
- **Large**: 명시적 AS-IS + PLAN, 독립 설계/contract review, 로컬 정책상 필요한 경우 Human 승인, 병합 전 독립 code review.
- **Epic**: 먼저 분할하고 integration ownership을 정의합니다. 하나의 거대한 implementation lane을 만들지 않습니다.

분석 중 더 높은 위험이 드러나면 grade를 올립니다.

## 제한된 AS-IS

다음 순서로 탐색합니다.

1. 직접 target path,
2. target symbol,
3. 직접 public contract,
4. 직접 caller/callee,
5. 관련 durable docs,
6. 구체적인 증거가 요구할 때만 더 넓은 검색.

관찰된 동작과 unknown을 기록합니다. 오래된 planning document를 현재 runtime truth로 취급하지 않습니다.

## TO-BE + 검증

Medium+ 작업은 다음을 정의합니다.

- 원하는 동작,
- 유지할 동작,
- failure behavior,
- 순서가 있는 change plan,
- acceptance criterion과 evidence의 매핑,
- 실제 경쟁 선택지가 있을 때만 real trade-off.

## Review / approval

설계 검토가 필요하면 제한된 phase-aware packet을 제공합니다. 환경이 immutable/revisioned artifact를 지원하면 review를 exact revision에 귀속합니다.

Review PASS는 Human 승인이 아닙니다.

Human 승인이 필요하면 실제 제시된 phase, artifact revision, decision scope에만 적용됩니다. 의미 있는 revision 변경에는 새 review/approval이 필요합니다.

## 구현

- 범위 안에서만 수정하고,
- 승인된 decision/contract를 유지하고,
- 관련 없는 cleanup을 피하고,
- 유용한 경우 직접 regression coverage를 추가합니다.

## 검증

대상 중심 check를 먼저 실행하고 변경 surface가 정당화할 때만 더 넓은 check를 수행합니다.

필수 check는 각각 PASS, FAIL, BLOCKED, UNVERIFIED로 보고합니다. 의도만으로 PASS를 추정하지 않습니다.

## 자체 검토

최종 diff를 다음과 비교합니다.

- 작업 항목 범위,
- 인수 조건,
- 경계,
- 승인된 decision,
- 의도하지 않은 변경.

## 완료

작업 완료를 선언하기 전에:

- runtime/product 동작이 바뀌면 durable docs를 갱신하고,
- 필요한 독립 검토를 완료하고,
- verification evidence와 unverified 항목을 기록하고,
- 저장소 정책에 따라 PR을 생성·갱신하고,
- 해당되는 경우 임시 artifact/workspace를 정리합니다.
