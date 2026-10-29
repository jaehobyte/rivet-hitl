---
name: hitl-architecture
description: "Use after human scope approval to design software architecture from evidence-backed functional requirements and prioritized six-part Quality Attribute Scenarios (QAS). Analyze quality trade-offs and tactics before choosing modules, patterns and interfaces. Never implement before human architecture approval."
---
# HITL Architecture v5 (QAS-first / ADD-lite)

목적: 요구사항을 함수·모듈로 곧바로 번역하지 않는다. **어떤 품질을 왜 더 중시하는지 → 어떤 설계 전략이 필요한지 → 어떤 모듈과 인터페이스가 적합한지**를 먼저 결정한다. 180분 해커톤에서는 SEI QAW/ADD/ATAM의 핵심 개념만 가볍게 적용한다. 전체 방법론의 정식 수행을 주장하지 않는다.

## 선행 조건과 입력
- Human Gate 2: 사용자가 Must/Should/Out과 남은 정책 위험을 실제로 승인했는가? 아니면 설계는 `PROPOSED`로 유지하고 승인 필요한 것을 표시한다.
- 실제 과제 README, `SPEC.md`, `QUESTION_BACKLOG.md`, `INTERVIEW_NOTES.md`, `DECISIONS.md`, 기존 코드·스키마·테스트·PG/외부 계약을 조사한다. 실제로 없는 파일·계약·수치를 있다고 가정하지 않는다.
- `references/QAS_PLAYBOOK.md`, `references/TRADEOFF_PLAYBOOK.md`를 **순서대로 읽어 이 Skill과 함께 적용**한다.
- 결과는 하나의 `ARCHITECTURE.md`로 충분하다. `templates/ARCHITECTURE.md`가 있다면 구조를 활용하되 기존 파일의 유효한 내용을 **삭제하지 않고 재구성**한다.

## 필수 실행 순서 — 모듈 설계는 7단계 이후
1. **System Context / Functional Drivers**: 사용자 목표, 핵심 Must FR, BR 불변조건, 실제 경계/통합 계약과 미해결 상태를 근거 ID와 함께 정리한다. 기능 정확성 자체와 품질 속성의 목표를 구분한다. `UNKNOWN` 보류 같은 업무 결과는 BR/FR이고, 장애 하에서도 보장하는 회복성과 상태 일관성은 QAS로 모델링 가능하다.
2. **Quality Attribute Inventory**: 신뢰성·정합성/복구성/가용성/성능/확장성/보안/사용성/변경 용이성/테스트 가능성 및 과제 특유 품질을 스캔한다. 각 속성에 `confirmed / candidate / unverified / N/A`, 업무 영향, 출처, 확인해야 할 측정조건을 적는다. 모든 속성을 필수 요구처럼 부풀리지 않는다.
3. **QAS 생성**: 아키텍처에 영향이 큰 품질 시나리오를 `QAS-001...`로 도출한다. QAS마다 **Source, Stimulus, Environment, Artifact, Response, Response Measure 6요소**와 요구사항/인터뷰 근거, 확인 상태를 반드시 적는다. 실제 수치가 없으면 임의 p95/TPS/가용성 비율 대신 `TBD(측정·협의 필요)`를 적는다. 무결성은 관찰 가능한 정수·불변조건 검증으로 명시할 수 있다.
4. **QAS 우선순위 / Utility Tree-lite**: 상쇄할 수 없는 **Hard Constraints**와 절충할 **Soft Quality Attributes**를 분리한다. 각 QAS에 `업무 중요도(H/M/L)`와 `설계 난이도·위험(H/M/L)`를 근거와 함께 부여하고 우선 QAS **3~5개 내외**만 심화한다(작은 과제는 더 적어도 됨). 중요도·위험을 근거 없이 수치화하지 않는다. **가중치가 현업/사람에게 실제로 주어졌을 때만** 정량 가중 점수 비교를 수행한다. Hard 조건 위반 후보는 가중 합산 전에 탈락.
5. **Architectural Tactics**: 상위 QAS 각각에 대해 대응 전술(예: 영속 유일성, 원자적 갱신, 장애 격리, 인증/인가, 측정·관측, 재시도 정책)을 제안하고 `QAS → tactic → 예상 부작용/도입 비용 → 검증 방법`으로 연결한다. 패턴 이름부터 정하지 않는다.
6. **Alternatives & Trade-offs**: 근거 있는 2~3개 후보(실제로 하나만 타당하면 그 이유)를 비교한다. 신뢰성↔지연, 확장성↔운영 복잡성, 보안↔사용성, 변경 용이성↔구현 시간 등의 충돌을 검토한다. 민감한 결정점(sensitivity), 여러 품질에 영향을 주는 trade-off point, 미검증 위험을 적는다. `references/TRADEOFF_PLAYBOOK.md`에 맞춰 탈락/선택 근거를 기록한다.
7. **Architecture / Module Design**: **선정된 tactics와 패턴**을 구현하기 위한 최소 모듈, 데이터 소유, 의존 방향, 포트/어댑터, 상태 전이, 트랜잭션/동시성 경계를 정의한다. `FR/BR/QAS → tactic → module/pattern` 연결이 없는 모듈은 만들지 않는다. 기존 코드 관례에 우선 정렬한다. 상세 함수 본문은 쓰지 않는다.
8. **Evaluation & Handoff**: 상위 QAS별로 `기대 결과 / 정량 또는 확인 가능한 판정 / 테스트 조건 / 실행 가능 여부 / 미검증 위험`을 정리한다. 아직 코드를 실행하지 않았다면 `설계상 예상/미검증`이지 PASS가 아니다. 데이터·AI 문제에서는 데이터 무결성, 누수, 재현성, 지연, 공정성(해당 시) 같은 QAS와 실제 측정 절차를 포함한다.
9. **Human Gate 3**: `ARCHITECTURE.md`에 `PROPOSED`, 주요 품질 우선순위, 핵심 설계 선택 3~5개, Trade-off, 미확정 사항을 기록하고 **사용자 승인을 기다린다**. 승인 전 제품 코드/의존성 설치/구현 Skill 호출 금지.

## 미해결 사안의 분기 (전체 중단 금지)
- `Policy Blocker`: 비즈니스 정책/허용 조건이 미정이면 **해당 기능의 동작을 정하지 않고** 사람 또는 권한자에게 질문 1개를 돌려준다.
- `Integration Blocker`: 실제 API·PG·DB 계약이 없으면 개념적 아키텍처와 추상 포트 설계는 가능하나, 실제 호환성과 end-to-end 보장은 **미검증**으로 둔다.
- `Architecture Assumption`: 내부 기술 선택을 위한 근거 있는 가정은 명시하고 대안/검증 계획을 붙인다. 가정으로 업무 정책을 대체하지 않는다.
- `QAS Gap`: 측정 수치/부하 조건이 없으면 후보 QAS의 measure는 `TBD`, 측정 계획을 작성한다. **그 결핍이 중요한 아키텍처 선택을 바꿀 때만** 기존 Interview Skill로 질문을 되돌린다.

## Hard Gates — 최종 셀프 체크
- [ ] 중요한 품질 속성 스캔을 모듈 설계보다 먼저 했는가?
- [ ] 우선 QAS에 6요소, 근거, 관찰 가능한 측정 방식이 있는가?
- [ ] Hard/Soft를 구분하고 실제 근거 없는 가중치·성능 수치를 만들지 않았는가?
- [ ] 상위 QAS → tactics → 대안 Trade-off → 선택한 패턴 → 모듈 → 검증이 연결되는가?
- [ ] 기존 코드/PG 계약이 없는데 운영 보장이나 통합 완료를 주장하지 않았는가?
- [ ] 승인 상태가 PROPOSED이며 사람에게 선택할 핵심 결정만 보고했는가?

## 출력
- `ARCHITECTURE.md`: **QA Inventory / QAS / Priority / Drivers / Tactics / Alternatives / Modules / Verification / Risks / Human approval** 순서.
- `DECISIONS.md`: 품질 우선순위, 선택·기각 대안, 사람의 명시적 승인(없으면 proposed).
- 화면에는 (a) 상위 QAS, (b) 설계 선택 3~5개와 Trade-off, (c) 필요한 역질문 0~1개, (d) 승인 여부만 간결히 요약한다.
- 제한시간이 촉박하면 `FAST` 모드: 핵심 2~3 QAS만 6요소로 완성하고 기존 기술 설계를 재활용한다. QAS 생성과 우선순위 단계를 **완전히 건너뛰지는 않는다**.
