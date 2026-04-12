# bid-pilot

> 수주 정보 분석 플러그인 — RFP 분석부터 경쟁사 매핑, 컨소시엄 전략, 입찰 자가평가, 문서 생성까지

공공조달, ODA, 민간 입찰 등 모든 유형의 입찰에 사용할 수 있는 범용 수주 지능 시스템입니다.
`_context/` 폴더에 회사 데이터(IR보고서, 과거 제안서 등)를 넣으면, 모든 분석에 회사 맥락이 자동 반영됩니다.

## 스킬 (8)

| 스킬 | 설명 |
|---|---|
| `context-loader` | 회사 데이터 자동 로딩 — _context/ 폴더 스캔, 프로필 구축 |
| `analyze-rfp` | RFP/TOR 분석 — 요구사항, 배점, 자격요건 추출 + 회사 데이터 대조 |
| `competitor-mapping` | 경쟁사 분석 — 정량 비교 매트릭스, 포지셔닝 전략 |
| `consortium-strategy` | 컨소시엄 전략 — 역할 배분, 지분율, 파트너 평가 |
| `bid-scoring` | 자가평가 — 근거 연결 채점, Gap 분석, Go/No-Go |
| `procurement-monitor` | 조달공고 모니터링 — 핵심정보 추출, 적합도 자동 평가 |
| `market-landscape` | 시장 환경 분석 — 트렌드, 발주처 전략, 기회 식별 |
| `export-deliverables` | 산출물 변환 — 마크다운을 HWP/PPT로 변환 (MCP 연동) |

## 커맨드 (6)

| 커맨드 | 체이닝 | 설명 |
|---|---|---|
| `/go` | — | 가이드 메뉴. 여기서 시작하세요 |
| `/bid-analyze` | context → rfp → competitor → consortium → scoring | RFP 수령 후 전체 수주 분석 |
| `/scan` | context → procurement-monitor → market-landscape | 공고 스캔 + 시장 맥락 분석 |
| `/compete` | context → competitor-mapping → consortium-strategy | 경쟁 분석 + 컨소시엄 전략 |
| `/pipeline` | context → rfp → competitor → consortium → scoring → 제안서 → 문서변환 | 전체 파이프라인 (RFP → HWP/PPT) |
| `/brief` | context → procurement-monitor (웹스캔) → 적합도평가 | 일일 브리핑 |

## 빠른 시작

```bash
# 플러그인 설치
claude plugin install bid-pilot

# 프로젝트 폴더에서 시작
/go
```

1. `_context/` 폴더에 회사 데이터를 넣으세요 (IR보고서, 과거 제안서 등)
2. `/go`를 실행하면 메뉴가 나타납니다
3. RFP를 첨부하거나 번호를 선택하세요

자세한 설치 방법은 [INSTALL.md](INSTALL.md)를 참조하세요.

## _context/ 폴더 구조

폴더 구조는 자유입니다. 파일을 넣기만 하면 AI가 내용을 분석합니다.
정리하고 싶다면 다음 구조를 권장합니다:

```
_context/
  company/       # 회사 소개, IR보고서, 신용평가
  proposals/     # 과거 제안서 (MD/PDF)
  credentials/   # 인증서, 수상 이력
  personnel/     # 핵심 인력 CV
  sector/        # 부서별 전문 데이터
    education/   #   교육: 강의SRT, 커리큘럼
    it/          #   IT: 기술명세, 아키텍처
    overseas/    #   해외: 국가별 조사자료
```

## 전체 파이프라인

```
RFP 수령
  ↓
/pipeline (또는 단계별 실행)
  ↓
[1] 회사 데이터 로드 (context-loader)
  ↓
[2] RFP 분석 (analyze-rfp) — 자격요건 자동 대조
  ↓
[3] 경쟁사 매핑 (competitor-mapping) — 정량 비교
  ↓
[4] 컨소시엄 전략 (consortium-strategy)
  ↓
[5] Go/No-Go 판단 (bid-scoring) — 근거 연결 채점
  ↓
[6] 제안서 초안 작성 → proposal-body.md
  ↓
[7] 문서 변환 (export-deliverables) → HWP / PPT
```

## MCP 서버 연동

문서 변환(Step 7)을 위해 다음 MCP 서버가 필요합니다:

- **hwpx_writer** — 마크다운 → HWP 문서 변환
- **pptx_writer** — 마크다운 → PPT 문서 변환

MCP 서버 설치 방법은 각 프로젝트의 README를 참조하세요.

## 사용 예시

```
# 처음 시작할 때
/go

# RFP를 첨부하여 전체 수주 분석
/bid-analyze [RFP 파일 첨부]

# 전체 파이프라인 (분석 → 제안서 → 문서)
/pipeline [RFP 파일 첨부]

# 조달공고 스캔
/scan [공고 텍스트 붙여넣기]

# 경쟁 분석
/compete 디지털 교육 혁신 사업

# 일일 브리핑
/brief

# 스킬 직접 사용 (자동 감지)
"이 KOICA 공고에서 평가기준 배점표를 정리해줘"
"예상 경쟁사를 분석해줘"
"컨소시엄을 어떻게 구성하면 좋을까?"
"이 분석 결과를 HWP로 변환해줘"
```

## 라이선스

MIT
