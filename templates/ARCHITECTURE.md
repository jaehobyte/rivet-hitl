# ARCHITECTURE — QAS-first Architecture (ADD-lite)

- 상태: `PROPOSED` (Human Gate 3 실제 승인이 있어야 HUMAN-APPROVED)
- 입력 근거: README, SPEC, 중요 INT-ID, 코드/DB/PG 자료, 인간의 범위 결정
- **기존 ARCHITECTURE.md가 있다면 유효한 모듈/상태 계약을 보존하면서 이 순서로 재배치한다.**

## 0. 인간이 1분 안에 확인할 핵심 요약
- 최우선 품질 속성 2~3개:
- 가장 중요한 Hard Constraint:
- 선택한 설계 전략 3개 / 이유:
- 반드시 확인할 미해결 위험:

## 1. Functional & Business Drivers
| FR/BR/CON ID | 필수 동작/불변조건 | 실제 근거 | 확정/미확정 |
|---|---|---|---|

## 2. Quality Attribute Inventory
| 품질 속성 | confirmed/candidate/unverified/N/A | 업무 영향·실패 비용 | 실제 근거 | 지표/측정 조건(TBD 허용) | 설계 영향 |
|---|---|---|---|---|---|

검토: 신뢰성·정합성, 복구성·가용성, 성능, 확장성, 보안·개인정보, 사용성, 변경 용이성, 테스트 가능성, 데이터·AI 특유 품질(해당 시). 관련 없는 속성은 N/A.

## 3. Quality Attribute Scenarios (6-part QAS)

<!-- 중요 시나리오마다 아래 양식을 복제. 요구사항 없는 임의 숫자 금지. -->
### QAS-[ID] · [품질 속성]
- 근거/상태: [FR/BR/NFR/INT/README; confirmed/candidate/open]
- **Source**:
- **Stimulus**:
- **Environment**:
- **Artifact**:
- **Response**:
- **Response Measure**: [실제 목표 또는 검증 가능한 불변조건; 미정이면 TBD와 측정계획]
- 관련 FR/BR/AC:

## 4. QAS Priority / Utility Tree-lite
| QAS ID | Hard/Soft | 업무 중요도 H/M/L (근거) | 설계 위험 H/M/L (근거) | 우선순위 / 선정 이유 |
|---|---|---|---|---|

- Hard(상쇄 불가) 조건:
- Soft(절충 가능) 품질:
- 가중치는 **사람/업무에서 제시했을 때만** 기록; 없으면 H/M/L. 판단을 실측값처럼 표시 금지.
- 깊이 있게 다룰 상위 3~5개 QAS(작은 과제는 축소 가능):

## 5. Architectural Tactics & Impact
| QAS-ID | Tactic (원리) | 도입 비용 / 다른 품질에 주는 영향 | 검증방법 | 근거/위험 |
|---|---|---|---|---|

## 6. Alternatives & Trade-offs (ATAM-lite)
| 후보 | Hard 조건 충족? | 상위 QAS 대응 | 비용/장점 | 민감한 결정점·Trade-off·미검증 위험 | 채택/탈락 |
|---|---|---|---|---|---|

- 최종 선택과 이유 (대안 비교 **후** 작성):
- Selection sensitivity / trade-off points:

## 7. Selected Architecture — Module, Patterns & Contracts
| Module/Port | 책임·소유 상태 | 의존/연결 인터페이스 | 채택한 Pattern | 관련 QAS/FR/BR |
|---|---|---|---|---|

- 최소 데이터 구조/키:
- 핵심 상태 전이/금지 전이:
- 트랜잭션·동시성 경계:
- 외부 시스템 계약(모르면 unknown/adapter only):
- 변경하지 않아도 되는 기존 코드:

## 8. Architecture Evaluation & Verification Handoff
| QAS / FR / AC | 설계상 기대 | 반증 가능한 테스트/실행 조건 | 측정 기준 | 설계 단계의 검증 상태 |
|---|---|---|---|---|

- `설계 검토`는 `실행 PASS`가 아니다. 실행·통합 전 항목은 NOT TESTED/UNVERIFIED로 둔다.

## 9. Open Decisions / Integration Blockers / QAS Gaps
| 항목 | Policy / Integration / Assumption / QAS Gap | 현재 영향·안전한 범위 | 해결 주체·필요 자료 |
|---|---|---|---|

## 10. Human Gate 3
- 승인 요청할 핵심 QAS 및 설계 결정:
- 사람의 명시적 승인/수정 내용/실제 근거:
- 상태: PROPOSED
