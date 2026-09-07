# 기존 저장소에 Adaptive Agentic SDD 도입하기

가장 안전한 도입 방식은 점진적입니다. 첫날부터 성숙한 프로젝트의 전체 workflow를 가져오지 마세요.

## Phase 1 — 수정 전에 관찰

제품 코드를 변경하지 않은 상태에서 에이전트에게 다음을 분석하게 합니다.

- 저장소·모듈 구조,
- 빌드와 테스트 진입점,
- 기존 CI·release 경로,
- 현재 issue/ticket 시스템,
- 아키텍처 또는 모듈 경계 규칙,
- 지속 관리되는 제품 문서,
- generated/vendor/shared 파일처럼 함부로 수정하면 안 되는 영역.

결과는 **관찰된 사실**과 제안하는 workflow 규칙을 구분해야 합니다.

## Phase 2 — 간결한 루트 agent contract 만들기

작은 `AGENTS.md`로 시작합니다.

- 현재 작업 항목을 식별하는 방법,
- 항상 적용되는 안전·범위 guard,
- 작업 라우팅,
- 빌드·테스트 명령 참조,
- 소수의 안정적인 아키텍처 불변식.

반복되는 모든 workflow 절차를 루트 파일에 넣지 않습니다.

`starter/AGENTS.md`는 형태를 참고하기 위한 것이며 그대로 복사하는 파일이 아닙니다.

## Phase 3 — 구현 Skill 하나 추가

하나의 canonical 구현 Skill로 시작합니다.

```text
작업 항목 -> 사전 점검 -> 등급 -> 제한된 분석 -> 계획 -> 구현 -> 검증 -> 검토 -> 완료
```

반복 운영 결과 분리가 컨텍스트를 줄이거나 모호성을 제거한다는 근거가 생긴 뒤에만 issue-management, parallel-development, finishing, QA 같은 Skill로 나눕니다.

## Phase 4 — 등급 정의

가장 작은 유용한 등급 체계를 사용합니다.

- Trivial,
- Small,
- Medium,
- Large,
- 저장소가 실제로 multi-lane 분할이 필요할 때만 Epic.

story point나 예상 코딩 시간만으로 분류하지 않습니다. 실패 비용, 되돌리기 어려움, contract surface, coordination risk를 기준으로 분류합니다.

## Phase 5 — 실제 tracker 연동

GitHub Issues는 필수가 아닙니다. Jira, Linear 또는 다른 tracker도 작업 항목 Source of Truth가 될 수 있습니다.

작업 시작 시 에이전트는 최소 다음 정보를 포함한 최신 ticket identity/revision 또는 신뢰할 수 있는 현재 snapshot이 필요합니다.

- 목표,
- 범위·제외 범위,
- 인수 조건,
- 의존성·blocker,
- 사용하는 경우 Priority·grade,
- 관련 linked spec.

에이전트가 tracker에 직접 접근할 수 없다면 오래된 복사본을 현재 티켓처럼 조용히 사용하지 마세요. snapshot의 시점과 경계를 명시합니다.

## Phase 6 — Trivial/Small 실제 작업으로 파일럿

Large migration을 시험하기 전에 몇 개의 좁은 실제 변경을 수행하며 workflow 자체를 관찰합니다.

- 에이전트가 관련 없는 파일을 읽었는가?
- 기존 로컬 규칙을 놓쳤는가?
- 범위 밖을 수정했는가?
- setup/install 작업을 불필요하게 반복했는가?
- 실행하지 않은 검증을 PASS로 보고했는가?
- PR이 증거를 명확히 설명했는가?

가상의 완전성을 위해 규칙을 추가하지 말고 관찰된 실패를 기준으로 workflow를 조정합니다.

## Phase 7 — 정당한 경우에만 깊은 게이트 추가

간결한 경로가 안정적으로 동작한 뒤 Medium/Large 메커니즘을 추가합니다.

- 명시적 AS-IS / TO-BE artifact,
- 독립 contract review,
- 리비전 귀속 Human 승인,
- 독립 code review,
- 누락·anchoring 위험을 위한 선택적 Blind Audit,
- 조건부 실기기·런타임 QA.

## Portable core와 로컬 safeguard

여러 저장소에서 일반화되는 규칙만 portable 방법론으로 승격합니다.

다음은 로컬에 유지합니다.

- 제품 전용 개인정보 제한,
- 모듈·경로 ownership,
- 빌드 도구 workaround,
- shell·encoding 특이사항,
- emulator/device bootstrap,
- 모델/provider routing,
- 조직 전용 승인 시스템.

이렇게 해야 성공한 한 저장소의 장애 이력이 다른 모든 프로젝트의 필수 의식으로 변하지 않습니다.

## 권장 첫 프롬프트

```text
제품 코드를 수정하지 말고 현재 저장소를 분석한다.

저장소 구조, 모듈 경계, build/test/CI 경로, 작업 항목 시스템, 기존 개발 규칙, 지속 관리 문서, 고위험 shared path를 식별한다.

그 다음 이 저장소에 필요한 최소 Adaptive Agentic SDD bootstrap을 제안한다. 성숙한 workflow를 통째로 복사하지 않는다. portable 규칙과 project-local 규칙을 분리하고, 어떤 규칙이 AGENTS.md, 작업 Skill, 조건부 docs에 들어가야 하는지 설명한다.
```
