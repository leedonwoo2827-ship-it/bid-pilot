---
description: RFP 분석부터 문서 생성까지 전체 수주 파이프라인을 순차 실행한다. 컨텍스트 로딩 → RFP 분석 → 경쟁 분석 → 컨소시엄 → 자가평가 → 제안서 작성 → 문서 변환.
argument-hint: "[RFP 파일 첨부]"
---

# /pipeline

RFP를 받아서 최종 제출 문서(HWP/PPT)까지 생성하는 전체 파이프라인.

## 실행 흐름

### Step 1: 컨텍스트 로딩 (context-loader)

`_context/` 폴더에서 회사 데이터를 로드한다.
- 회사 프로필, 수행실적, 인력 풀 구축
- 검증 리포트 출력 (보완 필요 항목 안내)
- _context/가 없으면 사용자에게 기본 정보 질문

### Step 2: RFP 분석 (analyze-rfp)

RFP/TOR 문서를 분석한다.
- 사업 개요, 배점표, 자격요건 추출
- 자격요건을 회사 데이터와 자동 대조
- 적합도 매트릭스 생성

**사용자 확인 후 다음 단계로 진행. 스킵 가능.**

### Step 3: 경쟁사 매핑 (competitor-mapping)

예상 경쟁사를 분석한다.
- 정량 비교 매트릭스
- 포지셔닝 전략

**사용자 확인 후 다음 단계로 진행. 스킵 가능.**

### Step 4: 컨소시엄 전략 (consortium-strategy)

최적의 컨소시엄을 설계한다.
- 단독 vs 컨소시엄 판단
- 역할 배분 및 지분율

**사용자 확인 후 다음 단계로 진행. 스킵 가능.**

### Step 5: 자가평가 (bid-scoring)

수주 가능성을 종합 평가한다.
- 배점표 기반 채점 (근거 연결)
- Gap 분석
- Go/No-Go 판단

**Go인 경우 다음 단계로 진행. No-Go면 중단 및 개선방안 안내.**

### Step 6: 제안서 초안 작성

bid-pilot 분석 결과를 바탕으로 제안서 초안을 작성한다.
- RFP 배점표 기반 섹션 구성
- _context/ 데이터에서 실적, 인력, 기술 역량 인용
- 색상 마커로 출처 표시

**산출물:** `proposal-body.md`

### Step 7: 문서 변환 (export-deliverables)

마크다운을 최종 문서로 변환한다.
- 사용자에게 형식 선택 요청 (HWP / PPT / 둘 다)
- hwpx_writer 또는 pptx_writer MCP 호출
- `_output/` 폴더에 저장

## 진행 원칙

- 각 단계 완료 후 사용자 확인을 받는다
- 어떤 단계든 스킵 가능 — 사용자가 "넘어가" 또는 "다음"이라고 하면 바로 다음 단계로
- Step 5에서 No-Go가 나오면: 개선방안을 안내하고, 그래도 진행할지 사용자에게 확인
- 정보 부족 시 가정을 명시하고 사용자에게 확인 요청

## 최종 산출물

```
프로젝트/
├── _analysis/
│   ├── rfp-analysis.md          (Step 2 결과)
│   ├── competitor-map.md        (Step 3 결과)
│   ├── consortium-strategy.md   (Step 4 결과)
│   ├── bid-scoring.md           (Step 5 결과)
│   └── pipeline-summary.md      (종합 보고서)
├── proposal-body.md             (Step 6 결과)
└── _output/
    ├── [사업명]-제안서.hwpx     (Step 7 결과)
    └── [사업명]-요약.pptx       (Step 7 결과)
```
