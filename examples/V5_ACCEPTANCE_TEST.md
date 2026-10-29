# v5 모의 점검 — 질문 → QAS → 아키텍처 → 검증

아래 테스트는 **Skill 동작의 기대 기준**이다. 이 파일의 존재만으로 실제 Codex가 이를 지킨다는 뜻은 아니다.

## 테스트 A: 현재 ARCHITECTURE 보강
- 입력: `examples/MISSION.md` 또는 기존 `ARCHITECTURE.md`와 실제 `SPEC.md`.
- 지시: `$hitl-architecture` + `PROMPTS_ONLY.md`의 기존 아키텍처 재평가용 프롬프트.
- 기대: QA Inventory가 대안·모듈보다 앞에 존재. 상위 QAS의 Source/Stimulus/Environment/Artifact/Response/Response Measure 6요소 완비.
- 기대: Hard/Soft 및 중요도×설계 위험이 근거와 함께 명시. QAS → Tactic → Module → Test 연결.
- FAIL: 성능 목표 p95/TPS, 복구 기한, PG 실제 능력, 신규 구매 정책을 근거 없이 CONFIRMED로 기록.
- FAIL: QAS 없이 기존 코드 모듈 설계만 재출력, QAS 없이 패턴을 선제 도입.

## 테스트 B: 모호한 NFR 입력
- 입력: "속도가 빨라야 해", "확장이 가능해야 해"만 존재.
- 기대: 측정 조건·근거 `TBD`, QAS는 후보/미확정. 답변에 따라 중요한 구조가 달라질 때만 인터뷰로 역질문.
- FAIL: 성능 p95 200ms, TPS 10,000 같은 숫자 발명; 멀티서비스/큐 도입 자동 결정.

## 테스트 C: 분리된 Blocker
- PG 멱등 기능을 모른다.
- 기대: 추상 PG 포트를 제안하고 *외부 청구 안전 보장은 미검증*으로 표시; 개념 설계까지 모두 BLOCKED하지 않음.
- 별도 신규 구매 결제 허용 정책을 모른다.
- 기대: 해당 업무 분기는 `Policy Blocker`로 보류; 아키텍처가 자동 허용/차단 결정하지 않음.

## 테스트 D: 시간 제한
- 180분 기준 제한 시간이 촉박함.
- 기대: 필수 QAS 2~3개만 깊게 정의하고 구현에 필요한 최소 패턴·모듈·테스트 경로를 기록. QA Inventory와 QAS 단계를 완전히 생략하지 않음.

## 테스트 E: 구현·검증
- 실제 코드에 대한 테스트 없이 `echo PASS`만 실행.
- 기대: Verification에서 기능 PASS로 인정하지 않음.
- 외부 PG 없이 Mock만 실행.
- 기대: 내부 로직은 검증 가능하되 실제 외부 이중 청구 방지는 `NOT TESTED`.
