# 설치 가이드

## 1. 플러그인 설치

### GitHub에서 설치 (권장)

```bash
claude plugin install bid-pilot
```

### 로컬 테스트

프로젝트 폴더를 다운로드한 후:

```bash
claude --plugin-dir ./bid-pilot
```

## 2. 회사 데이터 준비

플러그인이 회사 맞춤 분석을 하려면 `_context/` 폴더에 회사 데이터가 필요합니다.

### 최소 필요 데이터

| 파일 | 설명 | 예시 |
|------|------|------|
| IR보고서 또는 회사소개서 | 회사 개요, 재무, 조직 정보 | `회사소개서.pdf`, `IR보고서.pdf` |
| 과거 제안서 1건 이상 | 수행실적, 인력, 기술역량 | `OO사업-제안서.md`, `제안서.pdf` |

### 권장 추가 데이터

| 파일 | 효과 |
|------|------|
| 최근 3개년 재무제표 | 재무 적합도 정확한 판정 |
| 핵심 인력 CV | 인력 요건 자동 매칭 |
| 인증서/수상 이력 | 가산점 항목 자동 식별 |
| 부서별 전문 자료 | 분야 적합도 상세 분석 |

### 폴더 구조

파일을 `_context/` 폴더에 넣기만 하면 됩니다. 정리는 선택사항입니다:

```
프로젝트폴더/
└── _context/
    ├── 회사소개서.pdf         # 어디에 넣어도 OK
    ├── IR보고서.pdf
    ├── 과거제안서.md
    └── (기타 자료)
```

정리하고 싶다면:

```
프로젝트폴더/
└── _context/
    ├── company/              # 회사 소개, IR보고서, 신용평가
    ├── proposals/            # 과거 제안서
    ├── credentials/          # 인증서, 수상
    ├── personnel/            # 핵심 인력 CV
    └── sector/               # 부서별 전문 데이터
        ├── education/        #   교육: 강의SRT, 커리큘럼
        ├── it/               #   IT: 기술명세
        └── overseas/         #   해외: 국가별 자료
```

### 지원 파일 형식

- **마크다운 (.md)** — 가장 정확한 분석 (권장)
- **PDF (.pdf)** — 텍스트 추출 후 분석
- **텍스트 (.txt)** — 직접 읽기
- **SRT (.srt)** — 자막 텍스트 추출

## 3. MCP 서버 설정 (문서 변환용)

HWP/PPT 문서 변환이 필요하면 MCP 서버를 설치합니다.
분석만 하실 거라면 이 단계는 건너뛰어도 됩니다.

### hwpx_writer (HWP 변환)

```bash
# 설치
cd /path/to/hwpx_writer
pip install -r requirements.txt

# Claude Desktop 설정에 추가
# settings.json의 mcpServers에:
{
  "hwpx_writer": {
    "command": "python",
    "args": ["server.py"],
    "cwd": "/path/to/hwpx_writer"
  }
}
```

### pptx_writer (PPT 변환)

```bash
# 설치
cd /path/to/pptx_writer
pip install -r requirements.txt

# Claude Desktop 설정에 추가
{
  "pptx_writer": {
    "command": "python",
    "args": ["server.py"],
    "cwd": "/path/to/pptx_writer"
  }
}
```

## 4. 시작하기

```bash
# 프로젝트 폴더에서 Claude Code 실행
cd 프로젝트폴더

# 가이드 메뉴로 시작
/go
```

처음 실행하면 `_context/` 데이터를 확인하고, 부족한 항목이 있으면 안내합니다.

## 5. 일일 브리핑 자동화 (선택)

매일 아침 자동으로 조달공고 브리핑을 받고 싶다면:

```bash
# Claude Code에서 스케줄 설정
/schedule create --cron "0 8 * * 1-5" --prompt "/brief"
```

매 평일 오전 8시에 `/brief`가 자동 실행되어 브리핑 문서가 생성됩니다.

## 문제 해결

### _context/ 데이터가 인식되지 않을 때

- 파일이 `_context/` 폴더 안에 있는지 확인
- PDF 파일은 텍스트가 추출 가능한 형태인지 확인 (이미지만 있는 스캔 PDF는 OCR 필요)
- 마크다운 파일로 변환하면 분석 정확도가 높아집니다

### MCP 서버 연결이 안 될 때

- MCP 서버가 실행 중인지 확인
- Claude Desktop 설정의 경로가 올바른지 확인
- `pip install -r requirements.txt`로 의존성 설치 확인

### 메뉴로 돌아가고 싶을 때

언제든 `/go`를 입력하면 처음 메뉴로 돌아갑니다.
