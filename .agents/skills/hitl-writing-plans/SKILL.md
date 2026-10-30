---
name: hitl-writing-plans
description: "Use after human approval of SPEC.md and QAS-first ARCHITECTURE.md, before editing product code. Translate approved decisions into a small, executable PLAN.md with test-first steps, explicit interfaces, FR/BR/AC/QAS traceability, and human plan approval. Never implement in this skill."
---

# HITL Writing Plans — 승인된 설계를 실행 계약으로 변환

**책임:** `무엇을/왜/어떤 품질로 만들지`는 이미 승인되었다. 이 Skill은 **어떤 파일·인터페이스·테스트·순서로 구현할지**만 정한다. 실제 구현, 의존성 설치, 프로젝트 생성, 테스트용 제품 코드 작성은 하지 않는다.

**운용:** 전체 과정은 Spec-Driven Development(SDD)이며, 각 고위험 구현 Task는 Test-Driven Development(TDD)로 실행된다.

## 0. 선행 조건 / 자료 우선순위

- Human Gate 2의 범위 승인과 **Human Gate 3의 아키텍처 승인**이 명시적으로 존재해야 한다. 없으면 승인 전제와 `PROPOSED` 상태만 보고하고 구현 계획을 확정하지 않는다.
- **과제 `README.md` 및 제출 규칙**을 먼저 읽고, `SPEC.md`, `ARCHITECTURE.md`, `DECISIONS.md`, 사용 가능한 코드·테스트·스키마·빌드 설정을 확인한다. 필요한 경우 `QUESTION_BACKLOG.md`, `INTERVIEW_NOTES.md`의 출처도 확인한다.
- 실제 파일·테스트 프레임워크·패키지 매니저·API가 없다면 존재한다고 쓰지 않는다. 확인하지 못한 인터페이스는 `PROPOSED/PENDING`으로 표시하고 최초 Task에 환경 조사/계약 확인을 포함한다.
- 우선순위 충돌 시 공식 과제의 Must/제출 규칙 > 사용자가 실제 승인한 범위·정책 > 확인된 인터뷰·SPEC > 설계 가정. 충돌이 Must 동작을 바꾸면 사람에게 알리고 멈춘다.
- `ARCHITECTURE.md`의 Hard Constraints, 상위 QAS, 선택한 tactics, 상태/트랜잭션 경계, 미검증 운영 정책을 지킨다. 계획 과정에서 아키텍처를 임의 변경하지 않는다.

## 1. 범위·구현 가능성 스캔 (코드 작성 전)

1. **Repo reality check:** 실제 시작 명령, 테스트 명령, 배포/브라우저 연결 경로, 기존 모듈·함수 명칭을 확인한다. `README`가 지정한 필수 파일을 누락하지 않는다.
2. **Must-first:** Must 기능과 Hard NFR부터 작업 단위를 만든다. Should는 Must 완료에 필수인 경우만 포함하고 나머지는 별도 후보로 남긴다. Out은 구현하지 않는다.
3. **Blocking ledger:** `Policy Blocker`(업무 규칙 미정), `Integration Blocker`(실제 계약·데이터 없음), `Architecture Assumption`(검증할 기술 가정), `QAS Gap`(측정 목표 미정)을 구분한다.
   - `Policy Blocker`가 Must의 기대 동작을 바꾸면 해당 Task는 `BLOCKED`; 정책을 창작하지 않는다.
   - `Integration Blocker`는 허용된 mock/추상 계약으로 내부 부분을 계획할 수 있지만, 통합 성공을 미리 선언하지 않는다.
   - QAS의 `TBD`는 측정 준비 Task/검증 한계로 남기고 임의 수치나 PASS를 만들지 않는다.

## 2. 파일 구조를 Task보다 먼저 결정

- **File Map:** 생성/수정/테스트할 **실제 경로**와 파일별 단일 책임을 나열한다. 기존 프로젝트 관례를 우선하고 한 번에 바꿀 수 있는 범위를 좁게 유지한다.
- **Interface Contracts:** 인접 Task 사이에 공유할 함수/메서드 이름, 인자/반환 타입, API 요청·응답, 상태/오류, 영속성 경계만 정확히 적는다. 해당 저장소에서 확인되지 않은 이름은 가정임을 표시한다.
- 모듈·패턴·저장 경계는 승인된 `ARCHITECTURE.md`의 결정을 바꾸지 않는다. 세부 지역 함수는 구현 담당자가 결정하도록 둔다.
- 파일 경로와 인터페이스가 확정되지 않았으면 구현 Task를 작성하기 전에 필요한 **조회·검증 행동**을 먼저 명시한다. 없는 함수를 있는 것처럼 참조하지 않는다.

## 3. FR/BR/AC/QAS → Task → Test 매핑

- `SPEC.md`의 **모든 이번 범위 Must FR/BR/AC**와 `ARCHITECTURE.md`의 **우선 QAS**에 대해 반드시 하나 이상의 `Task ID` 및 검증 방법을 매핑한다. 빈 연결은 `GAP`으로 표시한다.
- QAS는 단순 함수 성공과 다를 수 있다. `Unit / Integration / Browser / Restart / Concurrency / Load / Security / Manual` 등 **검증 레벨**을 지정한다.
- `Review Focus`에는 **명세의 공백 때문에 사용자가 실패할 가능성이 높은 예외 3~5개**를 선정한다. 실패 입력, 기대 결과, 소유 Task, 테스트 이름을 함께 적는다. 예: 같은 요청 키·상이 payload, 인수인계 완료 직전 새로운 이슈, 재시작 직후 미해결 이슈 지속.
- 실제 외부 계약, 실데이터, 인증 정책을 확보하지 않은 경우 통합/보안/성능 테스트를 **NOT TESTABLE / PENDING**으로 분리하고 별도 테스트로 입증된 것처럼 취급하지 않는다.

## 4. Task 크기와 TDD 실행 명세

**하나의 Task = 독립적으로 관찰 가능한 결과물 + 자기 테스트 사이클 + 리뷰·거절할 수 있는 단위.** 단순 디렉터리 생성, 설정, 문서 작성은 관련 기능 Task에 흡수한다.

각 Task에는 최소 다음이 있어야 한다:

```markdown
### Task T-01: [결과물 중심 제목]
- Priority / depends_on: Must / 없음
- Covers: FR-..., BR-..., AC-..., QAS-...
- Files: Create ..., Modify ..., Test ... (실제 경로)
- Consumes / Produces: 정확한 계약·타입·상태·오류
- RED: 먼저 작성할 테스트 이름·대표 입력·assert와 실제 실행 명령; 기대 실패 원인
- GREEN: 통과할 최소 코드의 역할·파일 위치(본문을 통째로 작성하지 않음)
- REFACTOR: 불변조건 유지하며 개선할 점(필요할 때만)
- Verify: 실행 명령, 기대 결과, 회귀 범위, QAS 실측/수동 검증 분리
- Exit: 완료 조건, commit 가능 여부(허용 환경에서만)
```

- **RED는 '기대하던 기능이 없어서' 실패해야 한다.** 테스트 도구 누락, 문법 오류, 외부 서버 미가동을 정상적인 RED로 계산하지 않는다. 구현 전 테스트의 실패를 확인할 수 없는 상황은 `RED_NOT_RUN(reason)`으로 기록하며 가짜 TDD 증거를 만들지 않는다.
- **GREEN은 실제 명령을 실행한 결과**로만 인정한다. 고위험 상태 전이/경합/원자성/복구에는 정상과 반례 테스트를 함께 계획한다.
- **REFACTOR 후 관련 테스트를 다시 실행**한다. 도메인 계약을 바꾸는 리팩터링은 Human Gate로 돌린다.
- 단순 문서/UI/탐색처럼 TDD가 어울리지 않는 작업은 `NON_TDD`와 그 이유, 실제 검증 방법을 적는다. 무리하게 RED를 연출하지 않는다.
- Git commit은 허용된 저장소와 환경에서만 수행한다. 시험의 Git Push/외부 전송 제한을 준수한다.

## 5. PLAN.md 작성·Self-Review

프로젝트 루트에 **`PLAN.md` 하나만** 작성하거나 기존 것을 안전하게 갱신한다. 첫머리는 다음과 같이 간결하게 구성한다.

```markdown
# Implementation Plan — [과제]
> HITL: 승인된 계획을 `$hitl-implementation`이 순서대로 수행한다. 자동 서브에이전트는 사용하지 않는다.
- Status: PROPOSED (Human Gate 4 승인 대기)
- Source: README.md / SPEC.md / ARCHITECTURE.md / DECISIONS.md
- Goal / Architecture / Tech Stack / Available time
- Global Constraints: [과제 명시값만 그대로]
- Review Focus: [3~5개 실패 위험 + 소유 Task]
- Requirement & QAS Coverage: [ID → Task → test/method]
- File Map & Interfaces: [실제 경로, 계약]
- Task T-01 ... (RED → GREEN → REFACTOR → Verify)
- Pending / Excluded / Integration Limits
```

자체 검토 체크리스트:

- [ ] 모든 Must FR/BR/AC, 상위 QAS가 Task + 테스트/검증에 연결되었는가? `GAP`이 있는가?
- [ ] File Map을 Task보다 먼저 정했고, Task 간 타입·함수명·상태·오류 계약이 일치하는가?
- [ ] 각 Step에 **한 번의 행동과 확인 가능한 결과**가 있는가? `알아서 처리/TBD 테스트` 같은 모호한 지시는 없는가?
- [ ] `Review Focus`의 실패 위험마다 실제 검증 경로가 있는가?
- [ ] PLAN이 소스 코드의 장황한 복사본이 되지 않았는가? DRY/YAGNI를 지켰는가?
- [ ] 현실적인 시간에 Must 핵심 Vertical Slice를 먼저 실행할 수 있는가?
- [ ] 미확정 현업 정책·API·QAS 수치를 임의로 채우지 않았는가?

## 6. Human Gate 4 — 계획 승인 및 인계

- 계획을 `PROPOSED`로 보존하고, 화면에는 **Must Task 순서 / 우선 QAS 커버리지 / 선행 차단 사항 / 예상 검증 / 인간 결정 1~3개**만 요약한다.
- **사용자의 명시적 승인 전에는 구현을 시작하지 않는다.** 사용자가 수정 요청하면 PLAN만 고치고 재검토한다.
- 사용자가 계획을 승인하고 `hitl-implementation` 실행을 지시하면, 다음 Skill이 `PLAN.md`를 읽어서 수행한다. **본 Skill은 자동으로 다음 Skill을 호출하거나 서브에이전트를 생성하지 않는다.**
