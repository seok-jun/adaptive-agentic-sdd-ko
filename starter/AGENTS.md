# AGENTS.md — Starter

이 파일은 작게 유지해야 합니다. 항상 적용되는 guard와 작업 routing만 정의하고 반복 절차는 Skill이 소유합니다.

도입하기 전에 placeholder를 바꾸고 관련 없는 section은 삭제하세요.

## 시작 순서

1. 변경되지 않았다면 run당 이 파일을 한 번만 읽습니다.
2. 작업 항목 작업에서는 관련 없는 backlog보다 현재 Jira/GitHub/기타 ticket을 먼저 읽습니다.
3. 구현 작업은 `.agents/skills/implementing-issue/SKILL.md`로 라우팅합니다.
4. Skill 또는 현재 변경 surface가 요구할 때만 더 깊은 process/domain docs를 읽습니다.
5. 같은 run에서 변경되지 않은 context를 재사용합니다.

## 항상 적용 guard

- 관련 없는 사용자·팀 변경을 reset, clean, revert하거나 덮어쓰지 않습니다.
- 존재하지 않는 파일, API, label, module 이름, 명령, architecture rule을 만들어내지 않습니다.
- 관련 없는 refactor, rename, formatting sweep, dependency upgrade, feature를 현재 작업 항목에 섞지 않습니다.
- 명시된 수정 허용·금지 path 경계를 지킵니다.
- 필수 dependency, approval, ownership boundary를 안전하게 판단할 수 없으면 제품 수정은 중단하고 blocker를 보고합니다.
- 실행하지 않은 test/build/lint/runtime check를 PASS로 기록하지 않습니다.
- secret, production credential, private customer/user data를 commit, issue comment, PR, log에 남기지 않습니다.

## 작업 routing

| 작업 | Canonical path |
| --- | --- |
| 승인된 작업 항목 구현 | `.agents/skills/implementing-issue/SKILL.md` |
| Risk/grade semantics | `docs/sdd-workflow.md` |

반복 운영에서 실제 필요성이 확인된 뒤에만 Skill을 더 추가합니다.

## 프로젝트 불변식

안정적이고 가치가 큰 invariant만 기록합니다. 예:

- `<module A>`는 `<module B>`에 의존하면 안 된다.
- shared/generated/vendor path는 명시적 ownership이 필요하다.
- `<build command>`가 canonical targeted build entry point다.

이 section을 전체 architecture manual로 만들지 않습니다.

## 컨텍스트 효율

- 직접 path/symbol read를 우선하고,
- repository-wide dump를 피하고,
- 같은 run에서 변경되지 않은 work-item/rule context를 재사용하고,
- 성공한 command output은 요약하고,
- 실패 또는 구체적인 uncertainty가 있을 때만 확장하고,
- 토큰 절약을 위해 필수 증거를 포기하지 않습니다.
