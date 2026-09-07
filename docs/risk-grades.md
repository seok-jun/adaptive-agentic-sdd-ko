# 위험 등급

등급은 절차의 깊이를 제어합니다. 구현 시간의 추정치가 아닙니다.

## Trivial — Fast-track

다음 조건을 모두 만족할 때만 사용합니다.

- 런타임·제품 동작 변경 없음,
- test expectation, schema, dependency, build, API/contract, workflow 실행 의미 변경 없음,
- 대상이 직접적이고 좁음,
- 대상 static/path/format 검증만으로 충분함.

일반적인 예: 오탈자 수정, link/path 수정, 실행되지 않는 문서 정리.

기본 절차:

- 별도 SDD package 없음,
- 기본적으로 독립 검토 없음,
- 대상 검증,
- diff와 요청 범위 자체 검토.

실행 코드나 contract 의미가 바뀌면 수정 전에 최소 Small로 올립니다.

## Small — 간결(Lean)

변경이 하나의 구현 경계 안에 머물고 고위험 특성이 없을 때 사용합니다.

기본 절차:

- 작업 항목을 plan으로 사용할 수 있음,
- 제한된 대상·코드 검사,
- 대상 중심 검증,
- 자체 검토.

구체적인 이유가 있을 때 독립 검토를 선택적으로 적용합니다.

## Medium — 표준(Standard)

여러 구현 경계나 shared contract가 관련되지만 실패가 고비용 범주에 속하지 않을 때 사용합니다.

기본 절차:

- 명시적 AS-IS / TO-BE / change plan,
- 검증 전략,
- contract 또는 integration risk가 정당화할 때 제한된 독립 검토,
- 저장소 전체 재탐색 없음.

## Large — 심층(Deep)

다음과 같은 고영향 변경에 사용합니다.

- 보안·개인정보 경계 변경,
- 파괴적이거나 되돌리기 어려운 migration,
- cross-system consistency 또는 recovery logic,
- authentication·authorization,
- retry·scheduling semantics,
- cost·quota enforcement,
- 지속 공개 contract 변경,
- 이와 비슷하게 실패 비용이 큰 변경.

기본 절차:

- 명시적 AS-IS gate,
- 명시적 TO-BE/change-plan gate,
- 구현 전 독립 설계/contract review,
- 제품·아키텍처 결정이 로컬 정책상 Human-owned일 때 Human 승인,
- 더 넓은 검증,
- 병합 전 독립 code review.

누락·hidden assumption 위험이 중요하면 Blind Audit을 추가할 수 있습니다. 이는 escalation이며 portable core의 universal 요구사항이 아닙니다.

## Epic — 먼저 분할

모듈 경계를 바꾸거나, 모듈을 새로 만들거나, 독립적으로 전달 가능한 여러 capability를 포함하거나, 하나의 구현 lane으로 안전하게 추론하기 어려울 때 사용합니다.

기본 절차:

- 먼저 분할,
- integration ownership 정의,
- child work를 독립적으로 검증 가능하게 유지,
- 고위험 child/integration work에 Large 수준 엄격성 적용,
- Epic 전체를 소유하는 거대한 implementation worker 방지.

## Grade escalation

분석 중 작업 항목에서 보이지 않았던 위험이 드러나면 grade를 올립니다.

검토, 승인, 검증 비용을 피하기 위해서만 grade를 내리지 않습니다.
