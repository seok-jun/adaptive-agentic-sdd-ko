# 토큰·컨텍스트 효율

Adaptive Agentic SDD는 컨텍스트를 성실함의 증거가 아니라 예산으로 취급합니다.

## 점진적 공개(Progressive Disclosure)

다음 instruction 구조를 권장합니다.

```text
작은 루트 contract
  -> 작업 router
      -> canonical 작업 Skill
          -> 조건부 process/domain 문서
```

루트 contract에는 항상 적용되는 hard guard, 작업 routing, 소수의 안정적인 invariant만 둡니다. 반복 절차는 Skill·process 문서가 소유합니다.

## 기본 규칙

- 관련 없는 backlog보다 현재 작업 항목을 먼저 읽습니다.
- 저장소 전체 탐색보다 직접 path/symbol에서 시작합니다.
- 전체 파일 dump보다 선언부와 판단에 필요한 주변 줄만 확인합니다.
- 현재 path나 decision이 요구할 때만 domain/architecture/process 문서를 읽습니다.
- 같은 run에서 변경되지 않은 context를 재사용합니다.
- 작업 항목에서 이미 확정된 요구사항을 다시 탐색하지 않습니다.
- broad check보다 targeted check를 먼저 실행합니다.
- 성공 로그는 요약하고 실패한 경우에만 확장합니다.
- 전체 diff보다 changed-file 목록과 관련 hunk를 먼저 봅니다.
- review/QA agent에게 제품을 다시 설계하도록 요구하지 않습니다.
- 실제 경쟁 선택지가 없으면 trade-off 탐색을 만들지 않습니다.

## 증거를 최적화로 없애지 않기

토큰 절약을 이유로 다음을 제거해서는 안 됩니다.

- 필수 최초 read,
- scope·grade gate,
- 검증,
- grade가 요구하는 독립 검토,
- 정책이 요구하는 Human 승인,
- 인수 조건이 요구하는 runtime/device QA,
- 완료 증거.

규칙은 **증거를 줄이는 것이 아니라 관련 없는 컨텍스트를 줄이는 것**입니다.

## Same-run 재사용

하나의 run에서 planning -> implementation -> finishing으로 이동할 때 변경되지 않은 작업 항목 metadata, 루트 지침, source excerpt, verification state를 반복 조회하지 않고 재사용합니다.

## 프로젝트 로컬 예외

일부 저장소는 빌드 도구, shell, encoding, emulator, environment, 모델/provider routing에 추가 preflight가 필요합니다. 여러 프로젝트에서 일반화되기 전까지 이런 규칙은 로컬에 유지합니다.
