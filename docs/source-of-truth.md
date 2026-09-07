# 정보 유형별 Source of Truth

Adaptive Agentic SDD는 모든 정보에 하나의 전역 Source of Truth를 사용하지 않습니다.

질문의 종류에 따라 권위 있는 출처가 다릅니다.

| 정보 | 일반적인 기준 출처 |
| --- | --- |
| 현재 사용자 지시 | 현재 사용자·세션 요청 |
| 목표 / 범위 / 인수 조건 | 현재 작업 항목(GitHub Issue, Jira, Linear 등) |
| 전역 agent guardrail | 저장소 루트 agent policy |
| Workflow / gate | canonical workflow/process/Skill 정의 |
| 모듈 ownership / 의존성 규칙 | 아키텍처 경계 문서 |
| 현재 동작(AS-IS) | 현재 메인라인 코드와 직접 런타임 증거 |
| 구현 후 지속 제품 규칙 | 최종 코드에 동기화된 제품·비즈니스 문서 |
| UI 배치·상태 | 작업 항목에 연결된 visual specification |
| 과거 의사결정 근거 | Decision record / ADR |
| 임시 구현 계획 | SDD 작업 문서 |
| 검토 결과 | 가능한 경우 검토된 revision에 귀속된 review record |
| Human 승인 | phase + artifact revision + decision scope에 귀속된 승인 기록 |

## 왜 하나의 SSOT가 아닌가?

하나의 보편적 권위를 강제하면 레거시와 변화하는 시스템에서 오히려 충돌이 생깁니다.

예를 들어:

- **작업 항목**은 무엇이 바뀌어야 하는지 설명하고,
- **코드**는 현재 무엇이 실제로 동작하는지 설명하며,
- **아키텍처 정책**은 넘으면 안 되는 경계를 설명하고,
- **Visual Spec**은 UI 상태·배치를 설명하며,
- **review record**는 특정 검토 candidate에 대한 verdict를 설명합니다.

정보 영역이 명확하다면 이들은 서로 경쟁하는 출처가 아닙니다.

## 충돌 처리

**같은 정보 유형** 안에서 두 출처가 충돌하고 그 충돌이 범위, 아키텍처, 인수 조건, 승인 또는 지속 contract 의미에 영향을 주면 구현 전에 중단하고 해결합니다.

과거 댓글, 오래된 ticket 복사본, 이전 planning artifact, 이전 revision에 대한 review verdict를 사용해 현재 권위 있는 출처를 조용히 덮어쓰지 않습니다.
