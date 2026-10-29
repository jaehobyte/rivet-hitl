# Architecture Quality Gate — 사람의 2분 리뷰

- [ ] QA Inventory에 신뢰성·성능·확장성·보안·사용성·복구·변경 용이성 등 관련 속성이 평가되었나? (관련 없음은 N/A 가능)
- [ ] 상위 QAS에 Source/Stimulus/Environment/Artifact/Response/Response Measure 6요소가 모두 있나?
- [ ] Response Measure는 검증 가능한가? 수치가 미정이면 TBD와 측정방법이 있나?
- [ ] Must/Hard 조건이 Soft 점수로 상쇄되지 않았나?
- [ ] 가장 중요한 QAS 2~5개를 왜 골랐는지 근거가 있나?
- [ ] QAS → Tactic → Pattern/Module → Test가 추적 가능한가?
- [ ] 설계의 부작용과 어떤 품질을 포기했는지 Trade-off가 구체적인가?
- [ ] 실제 API/PG/DB 계약 부재를 운영 검증 완료로 포장하지 않았나?
- [ ] Q-019 같은 미확정 현업 정책을 아키텍트가 지어내지 않았나?
- [ ] 180분 내 구현 가능한 핵심 결정만 고정하고 나머지를 구현에 위임했나?

전부 YES일 필요는 없지만, NO는 원인·위험·판단 주체를 남겨야 한다. **사람의 명시적 승인 전 Implementation 시작 금지.**
