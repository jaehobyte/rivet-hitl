---
name: hitl-discovery
description: "Use when first reading a mission brief, README, code or dataset to extract evidence-grounded requirements and a complete decision-relevant question backlog before any stakeholder interview or coding."
---
# HITL Discovery (넓게 발견하되, 질문은 결정 영향으로 선별)

## 원칙
- 시작할 때 실제 과제 README, 제출 경로, 제약, 관련 코드·데이터, 기존 메모를 **먼저 조사한다**. 이미 찾을 수 있는 사실을 현업에 묻지 않는다.
- 확인된 내용(`CONFIRMED`), 추론(`HYPOTHESIS`), 질문 필요한 내용(`OPEN`), 비범위(`OUT`)를 엄격히 분리한다.
- 중요한 미정은 **개수 제한 없이 발견·저장**하되, 모든 미정을 현업에 질문하지는 않는다.
- 사람을 대신해 가상 페르소나에게 질문하거나 답을 지어내지 않는다. 이 Skill에서는 제품 코드를 구현하지 않는다.

## 실행 순서
1. **Mission Map**: 실사용자/업무 목표/현재 문제/기대 결과/필수 제출물/작업 시간/도구 제약/성공 기준을 요약한다. 데이터 중심 과제는 입력 데이터와 평가 지표도 확인한다.
2. **Evidence Inventory**: 각 주장에 `README 섹션, 코드 파일, 데이터 컬럼, 문서 인용` 등 확인 가능한 출처를 연결한다. 근거 없는 추론을 확정된 업무 정책으로 적지 않는다.
3. **Coverage Scan**: 목적·범위·도메인 용어·업무 흐름·정체성·상태·예외·외부 연동·핵심 NFR·검증 기준을 `Clear/Partial/Missing/N/A`로 점검한다. 데이터/AI형은 타깃, 라벨, 데이터 분할, 누수 가능성, Baseline을 포함한다.
4. **Question Opportunity 생성**: 답이 비어 있고 구체적인 결정·테스트 결과를 바꾸는 경우만 Q-ID로 생성한다. 질문의 순서는 광범위 → 구체적 예외이며, 중복을 합친다. 질문을 만드는 기준은 `references/QUESTION_CRAFTING.md`를 참고한다.
5. **라우팅**: `stakeholder(업무 권한) / source(제공 자료) / human(범위·위험 승인) / architecture(기술·구조 선택) / defer`로 분류한다. `source`는 가능한 한 즉시 확인한다. 현업 질문 후보는 **업무 담당자만 답할 수 있는 것**으로 한정한다.
6. **우선순위**: 답변 A/B가 `Must FR/BR/Hard NFR/중요 ARCH/AC`를 바꿔 **설계·핵심 테스트를 막으면 P0**. 주요 범위·품질을 바꾸면 P1. 그렇지 않으면 P2/유예. 거짓 확률이나 가짜 가중치를 만들지 않는다. `결정 차이(Decision Delta)`가 없다면 인터뷰 대상에서 제외한다.
7. **파일 갱신**: `QUESTION_BACKLOG.md`에 **전체 목록**과 그 근거·차이가 나는 결정·누가 답할지 저장한다. `SPEC.md`는 확정 사실과 가정·미확정 사항을 분리하여 초안 생성/갱신한다. 기존 파일의 Q-ID, 근거, 사용자 결정을 보존한다.
8. **출력**: 전체 항목 개수 및 P0 수, 지금 인터뷰할 우선 질문 **최대 3개 추천**(전체를 3개로 제한하지 않음), 질문별 왜 중요한지·무엇이 달라지는지 제시. 사람이 질문을 선택한 다음 **대기**한다.

## 질문 품질 게이트
- 'Kafka를 사용하나요?'와 같은 기술 유도 질문 금지. '실패한 이벤트가 중복 도착하면 어떤 업무 결과를 기대하나요?'처럼 동작을 묻는다.
- '모든 특수 케이스에 어떻게 대응하나요?' 같은 포괄 질문 금지. 결정에 중요한 상황 하나씩 구체화한다.
- README로 해결 가능한 사실, 스타일 취향, 중요도가 낮은 세부사항은 현업에게 묻지 않는다.
- 실제 현업 질문과 개발자가 결정할 기술적 의사결정을 섞지 않는다.

## Quality Attribute Discovery (아키텍처 설계 전 준비)
- `SPEC.md`에 **품질 속성 후보** 섹션을 추가: 신뢰성·정합성 / 복구성·가용성 / 성능 / 확장성 / 보안 / 사용성 / 변경 용이성 / 테스트 가능성 / 데이터·AI 고유 품질 중 이번 과제와 관련되는 것만 조사한다.
- 각 후보는 `증거 / confirmed·candidate·unknown / 예상 업무 영향 / 측정에 필요한 정보 / 설계가 달라질 수 있는 지점`을 기록한다. 근거 없는 목표 TPS, 응답시간, 가중치는 만들지 않는다.
- 질문이 실제 NFR 목표나 Hard Constraint를 바꾸고 현업 담당자가 답할 수 있을 때만 `QUESTION_BACKLOG.md`에 `stakeholder`로 넣는다. 기술 선택과 자료 미확인은 각각 `architecture`/`source`로 보낸다.
- 이 단계에서 6요소 QAS의 최종안이나 모듈 구조를 확정하지 않는다. `hitl-architecture`가 실제 근거를 가지고 QAS를 구체화한다.


## 종료 게이트
`QUESTION_BACKLOG.md`, `SPEC.md` 초안까지 작성하면 정지. 현업 답변을 만들지 않는다. 사용자에게 다음 행동을 명령하는 출력 필드를 추가하지 않는다. 답변을 확보한 뒤 `$hitl-interview`를 사용한다.
