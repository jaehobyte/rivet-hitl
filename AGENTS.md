# 프로젝트 공통 규칙

**목표:** 업무 담당자 인터뷰, 요구사항 확정, 품질 속성 기반 아키텍처 설계, 구현, 검증을 사람이 단계별로 통제한다. 어떤 Skill도 자동으로 다음 Skill을 실행하지 않는다.

## 단일 워크플로 (Human-in-the-Loop)
1. `$hitl-discovery`: 제공된 README/코드/데이터 조사. 중요한 미정 전체를 `QUESTION_BACKLOG.md`에 기록. `SPEC.md` 초안과 NFR 후보 생성. 구현 금지.
2. **Human Gate 1:** 사람이 현업 담당자에게 물을 질문을 선택하고 실제로 인터뷰한다.
3. `$hitl-interview`: 새 답변을 **먼저 `INTERVIEW_NOTES.md`에 원문 그대로 누적**, 다음 Backlog/Spec 갱신. 핵심 질문 반복.
4. **Human Gate 2:** 사람이 Must/Should/Out, 미해결 P0, 중요 NFR 후보와 범위를 승인한다.
5. `$hitl-architecture`: **QA Inventory → 6-part QAS → Utility Tree-lite Priority → Hard/Soft → Tactics → Alternatives/Trade-offs → Modules/Interfaces → QAS Verification** 순서. 앞 단계 생략 금지. `ARCHITECTURE.md`는 승인 전 PROPOSED. 기술 통합에 필요한 계약 부재와 개념 설계 가능성을 구분한다.
6. **Human Gate 3:** 사람이 QAS 우선순위, 주요 Trade-off, 최소 아키텍처 계약을 승인한다.
7. `$hitl-implementation`: 승인된 계약을 짧은 `PLAN.md`로 구현하고 FR/BR/QAS 테스트 실행.
8. `$hitl-verification`: Must 요구사항과 우선 QAS를 실제 증거로 검증. 최종 제출 판단은 사람.

## 전역 가드레일
- 실제 시험 README와 운영 규정이 최우선. 현업 정책·성능 수치·기존 API·PG 계약을 지어내지 않는다.
- FR/BR 불변조건과 품질 속성을 구분하되 QAS에서 연관 가능하다. Hard Constraints는 다른 품질 점수로 상쇄하지 않는다.
- 답변 하나가 무엇을 바꾸는지(Decision Delta)가 불명확하면 인터뷰 질문으로 억지로 만들지 않는다. 질문 목록은 무제한 생성해도 실제 인터뷰는 영향도 높은 것만.
- 업무 정책 미정 `Policy Blocker`, 실연동 불가 `Integration Blocker`, 명시적 `Architecture Assumption`, 미확정 수치 `QAS Gap`을 구분한다.
- 한 단계 완료 후 **반드시 인간이 다음 단계를 지시**한다. 사람의 승인을 추정하지 않는다.
- 구현 모델·추론 수준은 사용자가 제공된 Codex의 `/model`로 직접 선택한다. 자동 전환, 하위 에이전트, Python 엔진, JSON 레지스트리, Hook 의존 없음.
- 프로그램 실행, 테스트, 측정·브라우저 확인을 직접 하지 않았다면 PASS로 표기하지 않는다. 변경 후 관련 테스트 재실행.
- `DECISIONS.md`에는 AI 제안과 사람의 실제 결정(승인/기각)을 명확히 구분한다. 모든 문서/로그에는 과제 외의 개인 정보를 불필요하게 보존하지 않는다.

## 문서 계약
`QUESTION_BACKLOG.md` 전체 Q | `INTERVIEW_NOTES.md` 원문 append-only | `SPEC.md` 근거 있는 FR/BR/NFR | `ARCHITECTURE.md` QAS-first 기술 설계 | `PLAN.md` 수직 슬라이스와 테스트 | `DECISIONS.md` 인간 결정 | `VERIFICATION.md` 실행 증거.
