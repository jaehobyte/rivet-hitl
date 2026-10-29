---
name: hitl-implementation
description: "Use only after human approval of PLAN.md. Implement approved SPEC.md and QAS-first ARCHITECTURE.md task by task with real RED-GREEN-REFACTOR evidence, targeted regression tests, risk reporting and no autonomous redesign. Do not write a new plan or claim unverified completion."
---

# HITL Implementation — 승인된 PLAN을 TDD로 실행

**책임:** 이미 승인된 `SPEC.md → ARCHITECTURE.md → PLAN.md`를 코드와 테스트로 실현한다. **이 Skill은 계획을 새로 만들거나 아키텍처를 다시 선택하지 않는다.** Task별로 실제 RED → GREEN → REFACTOR를 반복하고 최종 검증은 `$hitl-verification`에 인계한다.

**운용:** Codex 단일 에이전트·Human-in-the-Loop 기본. 사용자가 `/model`에서 모델/추론 수준을 결정한다. 자동 서브에이전트 생성, 모델 변경, 외부 저장소 전송은 하지 않는다.

## 0. 실행 전 게이트

1. 실제 과제 `README.md` 및 제출 규칙, `SPEC.md`, `ARCHITECTURE.md`, **사용자가 승인한 `PLAN.md`**, `DECISIONS.md`, 현재 코드·테스트를 읽는다.
2. **Human Gate 3 및 Human Gate 4 승인 여부**를 확인한다. `PLAN.md`가 없거나 아직 `PROPOSED`이고 별도 명시적 승인이 없다면 코드를 수정하지 말고 `$hitl-writing-plans` 또는 사람에게 되돌린다.
3. 현재 브랜치·변경사항·테스트 명령·패키지 도구를 read-only로 조사한다. 기존 변경을 덮어쓰거나 다른 사람의 파일을 초기화하지 않는다. 실제 환경과 계획이 다르면 불일치를 보고하고 승인된 범위 안에서만 조정한다.
4. `Must`/Hard NFR, QAS의 수용 기준, Pending/Out 및 위험을 파악한다. **정책 미정 항목에 대한 동작을 만들어내지 않는다.**
5. Task 순서와 남은 시간을 확인한다. 우선 **끝까지 동작하는 Must Vertical Slice**를 만든다. 추가 프레임워크/리팩터링은 필요성을 입증하지 못하면 도입하지 않는다.

## 1. Task 단위 실행 루프 (핵심 동작)

각 Task를 시작할 때 `Covers: FR/BR/AC/QAS`, 파일·인터페이스, 테스트 레벨과 성공 조건을 한 번 확인한다. 이후 아래 순서대로 수행한다.

### A. RED — 기능이 없으면 실패하는 테스트

1. `PLAN.md`에 정해진 테스트 파일에 **기대 행위/불변조건을 먼저** 작성한다. 가능하면 명시적 예외·동시성·재시도·부분 실패를 포함한다.
2. 계획된 테스트 명령을 **실제로 실행**한다. 테스트 이름, 종료 코드, 실패 출력 중 핵심 부분을 확인한다.
3. **의도한 동작 미구현 때문에 실패했는지** 구분한다. Import 에러, 의존성 부재, 문법 오류, 테스트 탐색 실패는 RED가 아니다. 먼저 실행 환경/테스트 자체를 복구한다.
4. 테스트를 재현할 환경이 없다면 `RED_NOT_RUN`과 이유를 기록하고, 통과·TDD 수행을 주장하지 않는다. 허용된 범위에서 테스트 가능하도록 최소 셋업한다.

### B. GREEN — 가장 작은 올바른 구현

5. 승인된 모듈 경계·상태 전이·데이터 소유·트랜잭션·API 계약을 보존하면서 **테스트를 통과시키는 최소 코드**만 작성한다.
6. 관련 테스트를 다시 실행해 실제 `PASS`, 테스트 건수 및 종료 코드를 확인한다. 단순 `echo`, `exit 0`, 모의 성공 출력, 무관한 명령은 검증 증거가 아니다.
7. `PASS`여도 다른 Must 불변조건을 위반하지 않는지 확인한다. 관련 경계·회귀 테스트를 추가·실행한다. 코드를 고치기 위해 의미 있는 기대값을 낮추거나 테스트를 무력화하지 않는다.

### C. REFACTOR — 계약 보존 및 재실행

8. 구조가 지저분해졌을 때만 중복·가독성·불필요한 복잡성을 개선한다. 설계상의 핵심 책임이나 업무 정책 변경은 리팩터링으로 포장하지 않는다.
9. **리팩터링 뒤 관련 테스트를 새로 실행**한다. 실패하면 원인을 조사하고 동일 Task의 RED/GREEN 루프로 복귀한다.
10. `PLAN.md`에 Task 상태를 기록한다: `DONE | FAILED | BLOCKED | PARTIAL | NOT_TESTED`. `DONE`은 실제 산출물 + 새 테스트 증거 + 승인된 계약 준수 모두 충족할 때만 표기한다.

## 2. 주요 QAS / 검증 레벨 보존

- **추적성:** 계획의 `FR/BR/AC/QAS → Task → Test` 관계를 유지한다. 새로운 요구사항이 나오면 별도 기록 후 사람이 범위를 승인한다.
- **정합성/동시성:** 중복 입력, 같은 키·서로 다른 요청 내용, 동시 완료·종료, 오래된 화면·상태 충돌 등의 **반증 테스트**를 놓치지 않는다. 필요한 경우 트랜잭션/DB 실제 통합 테스트를 수행한다.
- **복구성:** 프로세스 재시작·중간 실패·저장 commit 경계는 단위 테스트만으로 운영 보장을 주장하지 않는다. 가능하면 재시작 통합 테스트를 한다.
- **사용성:** 핵심 화면·버튼·입력·차단 피드백을 실제 Browser에서 확인한다. UI 렌더링 성공은 사용자 작업 성공의 증거가 아니다.
- **성능/보안:** QAS 측정조건·운영 정책이 `TBD`인 부분은 측정 계획과 미검증 상태로 둔다. Mock·가상 데이터로 통합/권한/실데이터 검증 성공을 주장하지 않는다.
- **AI/데이터 과제:** 제공된 데이터와 평가 방식을 우선 읽고, 누수 없는 baseline/test를 구분한다. baseline과 개선 모델을 같은 평가 지표·조건으로 측정하고 결과를 기록한다.

## 3. 실패·설계 변경·시간 대응

- 테스트 실패 시 무작정 코드를 재작성하지 않는다. **실패 재현 → 원인 가설 → 최소 수정 → 동일 테스트 재실행**으로 복구한다.
- **경미한 구현 세부사항:** 승인된 인터페이스를 지키는 내부 변수명·보조 함수·파일 내부 구조 등은 스스로 결정 가능.
- **중요 계약 변경:** Must FR/BR, 상태·트랜잭션 경계, QAS Hard 조건, 외부 API/DB 호환성, 모듈 간 공개 계약을 바꾸어야 한다면 `문제/증거/변경안/영향받는 FR·QAS/재검증 계획`을 `DECISIONS.md`에 `PROPOSED`로 남기고 **사람 승인 전 관련 변경을 멈춘다**. 관련 없는 승인된 Task는 계속할 수 있다.
- **정책 또는 외부 자료 부족:** 실제 운영 보장을 임의 가정하지 않는다. `BLOCKED`, `INTEGRATION_PENDING`, `QAS_TBD`를 구별한다.
- **시간 부족:** Must의 테스트 가능한 핵심 경로와 제출 필수물을 먼저 완료한다. Should를 보류하더라도 자동으로 PASS 처리하지 않는다. 최종 검증 시간을 잠식하는 기능 추가를 멈춘다.
- **Git:** 허용된 환경에서 작업 단위로 커밋할 수 있지만, 시험 규정이 금지한 Git Push·외부 전송은 절대 하지 않는다. 금지되거나 불가능하면 변경 내역만 기록한다.

## 4. 완료 선언 전 Fresh Evidence Gate

완료라고 쓰기 전에 **현재 코드 기준**으로 아래를 수행한다.

1. **Identify:** 어떤 테스트·빌드·브라우저 조작이 어떤 Task/요구사항을 증명하는가?
2. **Run:** 해당 명령을 새로 실행한다. 통과한 옛 로그를 재사용하지 않는다.
3. **Read:** 종료 코드, 실제 테스트 건수/실패 건수, 중요한 출력, 실행 환경을 확인한다.
4. **Compare:** 기대 동작과 결과를 비교한다. 관련 변경 후 재실행하지 않았으면 `NOT_TESTED`다.
5. **Report:** Task별 상태를 `PLAN.md`에 기록하고, 검증 증거와 제한점을 남긴다. **부분 테스트 PASS != 전체 과제 완료**임을 명시한다.

기록 예시(실제 실행 전에는 빈 상태로 둔다):

```text
T-02 | FR-02/BR-05/QAS-001 | RED: failed as expected (test name) |
GREEN: test ... PASS (N tests, exit 0) | after refactor: PASS |
Changed: src/... , tests/... | Limit: actual equipment list unavailable
```

## 5. Human 인계

- 모든 승인된 Task의 `DONE/FAILED/BLOCKED/PARTIAL/NOT_TESTED` 및 변경 파일·실행 증거·남은 위험을 5~10줄로 요약한다.
- 실제 Browser/Build/전체 테스트가 아직 필요하면 누락 사실을 보고한다. 자기 보고만으로 최종 승인하지 않는다.
- 다음 단계는 **사람이 명시적으로 호출한** `$hitl-verification`. 독립 검토용으로 Sol 등 다른 모델을 쓸 수 있지만 사용자가 결정하며 자동 전환하지 않는다.
