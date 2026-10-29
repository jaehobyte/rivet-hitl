---
name: hitl-implementation
description: "Use after explicit human approval of ARCHITECTURE.md. Execute concise vertical-slice implementation plans with tests and actual run evidence, while preserving key module boundaries and policy contracts."
---
# HITL Implementation (승인된 구조를 빠르게 구현)

## 시작 전
- 시험 과제 README/제출물, `SPEC.md`, 승인된 `ARCHITECTURE.md`, 기존 코드, `DECISIONS.md`를 읽는다.
- 사람이 설계를 승인하지 않았으면 제품 코드 구현을 시작하지 않는다.
- 모델은 인간이 `/model`에서 변경한다. 잘 정의된 Task에는 제공 환경의 빠른 모델(Luna 후보)을 시도할 수 있고, 구조적 오류·동시성 문제는 추론 강한 모델(Sol 후보)로 되돌릴 수 있다. 실제 속도·정확성을 검증한 뒤 선택한다.

## 구현
1. `PLAN.md`에 수직 슬라이스를 나눈다(보통 3~7개, 강제 수 아님). 각각 `요구사항 ID → 파일·정확한 계약 → 검증 명령/기대 출력 → 완료 상태`를 포함한다.
2. 필수 작업부터 한다. 테스트 실패/현재 동작 증거를 확보한 뒤 최소 구현 → 실제 테스트/실행 → 결과 비교 → 수정/재실행한다.
3. 업무 정책, 키/상태·트랜잭션, 공개 API/모듈 경계, Hard NFR은 승인된 설계를 따른다. 지역 변수·사소한 내부 함수 등 세부만 독립 결정한다.
4. 데이터/AI 과제라면 데이터 탐색과 Baseline을 **실제로 실행**하고 같은 평가 설정에서 개선 값을 비교한다. 측정하지 않은 성능 수치를 쓰지 않는다.
5. 웹 UI가 있으면 서버를 실제로 띄우고 내장 Browser에서 주요 버튼·입력·이동을 확인할 수 있게 만든다. 단순 실행 성공 ≠ 기능 PASS.
6. 주요 아키텍처 계약이나 현업 불변조건을 변경해야 하면 `문제/근거/파급/최소 제안`을 기록하고 사람에게 보고한다. 동의 없이 재설계하지 않는다.

## QAS 계약 구현 — 변경 불가 조건
- `ARCHITECTURE.md`의 **우선 QAS-ID / tactic / 모듈 / 검증 조건**을 `PLAN.md`의 각 수직 슬라이스에 연결한다.
- Hard NFR/QAS를 구현하면서 위반하는 변경은 사용자가 명시적으로 승인하지 않는 한 수행하지 않는다. Soft 품질을 희생해야 한다면 Trade-off와 근거를 `DECISIONS.md`에 적고 사람에게 확인한다.
- 인터페이스만 맞아도 QAS를 충족하지 못할 수 있다. 정상 경로 외에 장애·동시성·복구 시나리오의 반증 테스트를 작성한다.
- QAS에 `TBD`가 있다면 수치를 상상해서 PASS를 만들지 말고 **측정 도구·부하 조건·확보할 데이터**를 계획에 적는다.
- 실제 PG와 연동되지 않은 Mock 테스트는 내부 로직 검증으로만 표기하고 외부 중복 청구 방지를 입증했다고 말하지 않는다.


## 종료
- `PLAN.md`에 완료·미완료·실행 명령·관련 파일을 기록한다.
- 완료 선언은 검증 단계에 맡긴다. 다음은 `$hitl-verification`이지만, 호출 여부는 사람이 정한다.
