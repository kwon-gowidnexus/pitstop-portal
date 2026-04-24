# 피트스탑 대시보드 v3 — 디자인 스펙

> 작성: 2026-04-06 | 담당: Designer
> 목적: MD 파일(피트스탑_준비_*.md)의 풍부한 분석 내용을 고밀도 IB 터미널 스타일로 시각화
> 기반: pitstop-portal/v4/index.html 디자인 시스템 그대로 계승

---

## 1. 레이아웃 와이어프레임

### 1-1. 전체 구조

```
┌─────────────────────────────────────────────────────────────────────┐
│ TOPBAR (46px)                                                        │
│  PITSTOP PORTAL  ·  대시보드 v3  [INTERNAL]           04-06 / S2    │
├────────────┬────────────────────────────────────────────────────────┤
│  SIDEBAR   │  MAIN (scroll)                                         │
│  (280px)   │                                                        │
│            │  ┌── A. COMPANY HEADER ──────────────────────────────┐ │
│  [검색]    │  │  회사명  CEO  설립  ·  메타 필  RM↗  단계바        │ │
│            │  └──────────────────────────────────────────────────┘ │
│  ─────────  │                                                        │
│  01 회사A  │  ┌── B. VERDICT STRIP ────────────────────────────────┐ │
│  [RED]     │  │  ● RED  "부채 20억 정체 미확인 + 실매출 490만원"    │ │
│            │  └──────────────────────────────────────────────────┘ │
│  02 회사B  │                                                        │
│  [YELLOW]  │  ┌── C. GATE PANEL (7 gates) ──────────────────────┐  │
│            │  │  G1⚠  G2❌  G3❌  G4⚠  G5⚠  G6⬜  G7❌          │  │
│  03 회사C  │  │  [각 게이트 카드 상세]                             │  │
│  [GREEN]   │  └──────────────────────────────────────────────────┘ │
│            │                                                        │
│  ──────── │  ═══ DIVIDER ══════════════════════════════════════════ │
│  [CLOSED] │                                                        │
│  04 회사D  │  ┌── D. FINDINGS STRIP ──────────────────────────────┐ │
│  [NOGO]   │  │  신규 발견사항 1~N (번호 리스트, 압축 표시)          │ │
│            │  └──────────────────────────────────────────────────┘ │
│            │                                                        │
│            │  ═══ DIVIDER ══════════════════════════════════════════ │
│            │                                                        │
│            │  ┌── E. FINANCIALS (탭 전환) ────────────────────────┐ │
│            │  │  [BS] [현금추이] [DW현금흐름] [입출금상세] [인원]   │ │
│            │  │  ─────────────────────────────────────────────    │ │
│            │  │  (선택된 탭의 테이블/미니차트)                      │ │
│            │  └──────────────────────────────────────────────────┘ │
│            │                                                        │
│            │  ═══ DIVIDER ══════════════════════════════════════════ │
│            │                                                        │
│            │  ┌── F. RED FLAGS ────────────────────────────────────┐ │
│            │  │  [CRITICAL] [HIGH] [MEDIUM] 섹션별 플래그 리스트   │ │
│            │  └──────────────────────────────────────────────────┘ │
│            │                                                        │
│            │  ═══ DIVIDER ══════════════════════════════════════════ │
│            │                                                        │
│            │  ┌── G. BEP SCENARIO ─────────────────────────────────┐ │
│            │  │  현재상태 수치 + 시나리오 테이블 + 런웨이 계산       │ │
│            │  └──────────────────────────────────────────────────┘ │
│            │                                                        │
│            │  ═══ DIVIDER ══════════════════════════════════════════ │
│            │                                                        │
│            │  ┌── H. KHK DECISION ────────────────────────────────┐ │
│            │  │  GO / NO-GO / 보류 시나리오 3컬럼                   │ │
│            │  └──────────────────────────────────────────────────┘ │
│            │                                                        │
│            │  ═══ DIVIDER ══════════════════════════════════════════ │
│            │                                                        │
│            │  ┌── I. NEXT ACTIONS ─────────────────────────────────┐ │
│            │  │  우선순위 테이블 + 분기 다이어그램(ASCII)            │ │
│            │  └──────────────────────────────────────────────────┘ │
│            │                                                        │
│            │  ┌── J. EMAIL DRAFT ──────────────────────────────────┐ │
│            │  │  접힌 상태(collapsed). 클릭 시 full 텍스트 노출     │ │
│            │  └──────────────────────────────────────────────────┘ │
└────────────┴────────────────────────────────────────────────────────┘
```

### 1-2. 정보 우선순위 (스크롤 없이 핵심 파악)

| 화면 내 위치 | 섹션 | 의도 |
|------------|------|------|
| viewport 상단 | A(Header) + B(Verdict) + C(Gate 7개) | 30초 내 GO/NOGO 판단 가능 |
| 스크롤 1단계 | D(발견사항) + E(재무 탭) | 사실 근거 확인 |
| 스크롤 2단계 | F(Red Flags) + G(BEP) | 리스크 정량화 |
| 스크롤 3단계 | H(KHK 판단) + I(다음 액션) + J(이메일) | 의사결정 지원 |

---

## 2. 색상 / 타이포 시스템

### 2-1. 기존 토큰 계승 (변경 없음)

```css
/* 기반 */
--bg:         #ffffff
--surface:    #f8f9fa
--surface2:   #f1f3f5
--border:     #e5e7eb
--border-mid: #d1d5db
--text-pri:   #111827
--text-sec:   #4b5563
--text-dim:   #9ca3af
--mono:       'Noto Sans Mono', monospace
--sans:       'Noto Sans KR', sans-serif
--r:          4px

/* 상태 색상 */
--c-green:    #16a34a  / --bg-green:   #dcfce7
--c-yellow:   #ca8a04  / --bg-yellow:  #fef9c3
--c-red:      #dc2626  / --bg-red:     #fee2e2
--c-orange:   #ea580c  / --bg-orange:  #ffedd5
--c-unknown:  #9ca3af  / --bg-unknown: #f3f4f6
```

### 2-2. v3 추가 토큰

```css
/* Verdict 스트립 배경 */
--verdict-red-bg:    #fff0f0     /* --bg-red보다 연한 버전 */
--verdict-yellow-bg: #fffbeb
--verdict-green-bg:  #f0fdf4

/* 섹션 구분선 */
--divider-bg: #f1f3f5            /* section-divider: 6px 높이 */

/* Flag 심각도 */
--flag-critical-bg:  #fee2e2     /* --bg-red */
--flag-critical-fg:  #dc2626     /* --c-red */
--flag-high-bg:      #ffedd5     /* --bg-orange */
--flag-high-fg:      #ea580c     /* --c-orange */
--flag-medium-bg:    #fef9c3     /* --bg-yellow */
--flag-medium-fg:    #ca8a04     /* --c-yellow */

/* 현금 잔고 미니차트 바 */
--bar-critical: #dc2626          /* 잔고 < 1억 */
--bar-warning:  #ea580c          /* 1억 ≤ 잔고 < 5억 */
--bar-normal:   #16a34a          /* 잔고 ≥ 5억 */

/* Gate 상태 아이콘 색 */
--gate-pass:    #16a34a          /* ✅ */
--gate-warn:    #ca8a04          /* ⚠️ */
--gate-fail:    #dc2626          /* ❌ */
--gate-blank:   #9ca3af          /* ⬜ (미확인) */
```

### 2-3. 타이포그래피

| 용도 | 폰트 | 크기 | 굵기 | 색상 |
|------|------|------|------|------|
| 회사명 | Noto Sans KR | 16px | 600 | --text-pri |
| 섹션 타이틀 | Noto Sans KR | 13px | 700 | --text-pri |
| Gate 항목명 | Noto Sans KR | 11px | 600 | --text-pri |
| 본문/소견 | Noto Sans KR | 12px | 400 | --text-sec |
| 수치 데이터 | Noto Sans Mono | 12px | 400 | --text-pri |
| 음수/감소 수치 | Noto Sans Mono | 12px | 500 | --c-red |
| 양수/증가 수치 | Noto Sans Mono | 12px | 500 | --c-green |
| 레이블/캡션 | Noto Sans Mono | 9px | 400 | --text-dim (uppercase) |
| 배지 텍스트 | Noto Sans Mono | 8px | 500 | 상황별 |
| 이메일 본문 | Noto Sans KR | 12px | 400 | --text-sec |

---

## 3. 섹션별 컴포넌트 스펙

### A. Company Header

**구조**:
```
[좌] 회사명(16px/600) + CEO pill + 설립 pill + 사업설명(12px/sec)
     고위드: 한도 pill + 리스크등급 pill + 연체이력 pill(red)
[우] [CF RM ↗] 버튼 (기존 btn-rm 스타일) + [이메일 초안 ↓] 버튼
```

**메타 pill 목록**: `.co-stat-pill` (기존 클래스 재사용)
- CEO: `반성훈`
- 설립: `2019-11-14`
- 누적투자: `61.5억 · 4회`
- 마지막투자: `2022-05 Series A (롯데벤처스)`
- 연체이력 pill: 36회는 `.co-stat-pill` + `border-color: --c-red` + `color: --c-red` 처리

**Stage 진행 바**: 기존 `.stage-bar` 컴포넌트 그대로. S1~S5 표시, 현재 단계 current 상태.

**간격**: padding 18px 24px 16px, border-bottom 1px solid --border

---

### B. Verdict Strip

**목적**: 스크롤 없이 Overall 판단을 즉각 인식

**구조**:
```
┌─────────────────────────────────────────────────────────────────┐
│  ● RED   "부채 20억 정체 미확인 + 현금 0.5억 + 실매출 490만원"  │
│          Stage 2 최우선: 부채 20억 구성 + 실매출 확인           │
└─────────────────────────────────────────────────────────────────┘
```

**스타일 규칙**:
- 배경: verdict에 따라 `--verdict-red-bg / yellow-bg / green-bg`
- 좌측 4px 컬러 보더: `border-left: 4px solid --c-red/yellow/green`
- 도트 아이콘: 8px 원, 대응 색상 fill
- 판정 레이블: Mono 11px 500 uppercase (`RED` / `YELLOW` / `GREEN`)
- 1줄 요약: Sans 13px 600 --text-pri
- 2줄 보충: Sans 12px 400 --text-sec
- padding: 14px 20px, margin: 0 (header와 gate 사이 구분선 없음, 자체 배경으로 구분)

---

### C. Gate Panel

**목적**: G1~G7 상태를 한눈에, 각 소견을 즉시 확인 가능

**레이아웃**: 7개 카드를 4+3 또는 7 단일 행으로 나열
- 권장: `grid-template-columns: repeat(7, 1fr)` — 화면 너비에 맞게 꽉 채움
- 좁을 경우 fallback: `repeat(4, 1fr)` + 아래 `repeat(3, 1fr)` 2행

**Gate 카드 구조** (`.gate-card`):
```
┌──────────────────────────────────┐
│ [상단 색 보더 3px]               │
│  G1    피트스탑 적합성    [⚠️]   │
│  ─────────────────────────────   │
│  "운영 안정화 의지 있으나         │
│   구체성 부족"                   │
└──────────────────────────────────┘
```

**스타일**:
- 기존 `.crit-card` 패턴 계승 (border-top 3px 컬러 + surface 배경)
- 상단 보더색: gate 상태에 따라 `--c-green / --c-yellow / --c-red / --c-unknown`
- Gate ID: Mono 9px --text-dim uppercase (`G1`)
- 항목명: Sans 11px 600 --text-pri
- 상태 아이콘: 16px 텍스트 아이콘 (❌ ⚠️ ✅ ⬜) — 우측 정렬
- 소견 텍스트: Sans 11px 400 --text-sec, `line-height: 1.5`
- padding: 12px 13px 10px
- 미확인(⬜) 카드: `--c-unknown` 보더, 소견란에 `"사전미팅에서 확인"` 이탤릭 처리

**Gate 상태 뱃지** (`.gate-status`):
- ❌ FAIL: bg `--bg-red`, color `--c-red`
- ⚠️ WARN: bg `--bg-yellow`, color `--c-yellow`
- ✅ PASS: bg `--bg-green`, color `--c-green`
- ⬜ TBD: bg `--bg-unknown`, color `--c-unknown`
- Mono 8px 500 uppercase, padding 2px 6px, border-radius 3px

---

### D. Findings Strip

**목적**: 신규 발견사항을 스캔 가능한 리스트로

**구조**:
```
FINDINGS — 신규 발견사항  [9건]
──────────────────────────────────────────────────
  1  재무입금 전액 보조금 — 25.07 이후 합산 ~10.5억. 차입/증자 0.
  2  현금흐름표 26.03 실매출 490만원 — 정부지원금 2.14억이 수입 97.8%.
  3  대여금 5,000만원 유출 — 수취인 불명. 확인 필수.  [RED]
  ...
```

**스타일**:
- 섹션 타이틀: Mono 9px uppercase --text-dim + 카운트 배지
- 각 항목: `display: flex; gap: 12px; padding: 8px 0; border-bottom: 1px solid --border`
- 번호: Mono 10px --text-dim, 우측정렬, `min-width: 20px`
- 본문: Sans 12px --text-sec
- 핵심 수치는 `<strong>` (Mono 12px 600 --text-pri)
- 긴급 항목에 우측 `[RED]` / `[HIGH]` 뱃지 표시 (선택, flag 섹션과 연동)

---

### E. Financials (탭 전환)

**탭 목록**:
1. BS — 재무제표 3개년
2. 현금 잔고 — 월별 추이 + 미니 바차트
3. DW 현금흐름 — 월별 RCF 테이블
4. 입출금 상세 — 제출 현금흐름표
5. 인원 추이 — 혁신의숲 데이터

**탭 바 스타일** (`.fin-tabs`):
- 탭 아이템: Mono 10px --text-dim, padding 8px 14px
- 활성 탭: --text-pri + border-bottom 2px solid --text-pri
- 배경: --surface, border-bottom: 1px solid --border
- 탭 전환: JS로 div show/hide (CSS class toggle)

#### E-1. BS 테이블

**컬럼**: 항목 | FY2022 | FY2023 | FY2024 | 추세

**행 그룹** (시각적 구분선으로 그룹화):
- 손익: 매출, 영업이익, 순이익
- 재무상태: 총자산, 현금, 자본, 부채
- 부채 상세: 단기차입, 이자비용, 누적적자
- 비용: 인건비, 임차료, 수수료

**스타일 규칙**:
- 행 그룹 첫 행 위: `border-top: 2px solid --border-mid`
- 음수 수치: `--c-red`
- 핵심 이상 수치 (단기차입 20억, 이자비용 급증 등): bg `#fff8f8` + bold
- "추세" 컬럼: 텍스트 화살표 (`↑↑` / `↓` / `→`) + 12px sans --text-sec

#### E-2. 현금 잔고 추이

**상단**: 현재 잔고 KPI 카드 (대형 수치)
```
┌──────────────────────────────────────────┐
│  현재 잔고  [0.50억]  [CRITICAL]          │
│  피크 대비  -96.9%  (16.0억 → 0.50억)    │
│  런웨이    0.3개월 (보조금 없을 시)        │
└──────────────────────────────────────────┘
```

**바 차트** (CSS-only, 인쇄 친화적):
- 각 월을 `display: flex; align-items: flex-end` 컨테이너 안의 수직 바로 표시
- 바 높이: `height: calc(잔고 / 최대잔고 * 80px)` — 인라인 스타일
- 바 색상: 잔고 기준으로 --bar-critical / --bar-warning / --bar-normal
- 월 레이블: Mono 8px --text-dim, 하단 표시 (45° 회전)
- 수치 레이블: 바 상단에 Mono 9px, 주요 변곡점만 표시

**하단**: 전체 월별 테이블 (컬럼: 월 | 잔고 | 전월대비 | 비고)
- 전월대비 음수: --c-red, 양수: --c-green

#### E-3. DW 현금흐름

**테이블 컬럼**: 월 | 매출입금 | 영업출금 | OCF | 급여 | 재무입금 | 보조금 | 대출상환 | 비고

**스타일 규칙**:
- OCF 열: 음수 셀 bg `#fff8f8`, 양수 셀 bg `#f0fdf4`
- 보조금 열: 값이 있으면 bg `--bg-yellow` (시각적 주의)
- 급여 이상 셀 (26.01 = 0.04억): bg `--bg-red`, bold
- 비고란: Mono 9px --text-dim

#### E-4. 입출금 상세

**2개 서브 테이블**: 입금 상세 | 출금 상세 (좌우 또는 상하 배치)

입금 테이블 컬럼: 항목 | 26.02 | 26.03 | 비고
출금 테이블 컬럼: 항목 | 26.02 | 26.03 | 비고

**스타일 규칙**:
- 정부지원금 행: bg `--bg-yellow`, Mono bold — 의존도 강조
- 실매출 소계 행: bg `--bg-red` + bold — 490만원 가시화
- 대여금 행: bg `--bg-orange`, 우측 `[확인필수]` 뱃지
- 법무비용 행: bg `--bg-orange`

#### E-5. 인원 추이

**KPI 상단**: `피크 31명 → 현재 22명 (-9명, -29%)`

**테이블**: 월 | 인원 | 입사 | 퇴사 | 비고
- 퇴사 수 ≥ 3: --c-red
- 현재 행: bold + left border 3px --text-pri

---

### F. Red Flags

**구조**: CRITICAL → HIGH → MEDIUM 순서, 각 등급은 별도 블록

**등급 헤더** (`.flag-section-head`):
```
● CRITICAL  3건
─────────────────────────────────────────
```
- 도트: 10px 원, 등급 색상
- 레이블: Mono 10px 600 uppercase, 등급 색상
- 카운트: Mono 9px --text-dim

**플래그 카드** (`.flag-card`):
```
┌─ [CRITICAL] ──────────────────────────────────────────────────────┐
│  FLAG 1  실매출 급감 + 보조금 의존 97.8%                           │
│  ──────────────────────────────────────────────────────────────    │
│  "현금흐름표 26.03: 실매출 490만원, 정부지원금 2.14억(97.8%)"      │
│  "보조금 종료 시 즉시 파산 구조"                                    │
└───────────────────────────────────────────────────────────────────┘
```

**스타일**:
- 좌측 4px 보더: 등급 색상
- 배경: 등급 bg 색상 (연한 tint)
- 등급 뱃지: Mono 8px 500 uppercase, 등급 색상 fg/bg
- 플래그 번호: Mono 9px --text-dim
- 제목: Sans 12px 600 --text-pri
- 본문: Sans 12px 400 --text-sec, `line-height: 1.6`
- 핵심 수치 강조: `<strong>` Mono 12px 600
- 카드 간격: `margin-bottom: 8px`
- 등급 섹션 간격: `margin-bottom: 20px`

---

### G. BEP Scenario

**구조 3단계**:
1. 현재 상태 KPI 그리드 (4칸)
2. 시나리오 테이블
3. 투입금액별 런웨이 테이블

**현재 상태 KPI 그리드** (`.bep-kpi-grid`, 4컬럼):
```
┌──────────┬──────────┬──────────┬──────────┐
│  월 매출 │ 월 burn  │  현금    │  런웨이  │
│  490만원 │  1.68억  │  0.5억   │  0.3개월 │
│ [CRITICAL│[WARNING] │[CRITICAL]│[CRITICAL]│
└──────────┴──────────┴──────────┴──────────┘
```
- 수치: Mono 18px 600 --text-pri (또는 등급 색상)
- 레이블: Sans 10px --text-dim
- 등급 뱃지: 하단, Mono 8px
- 카드: surface 배경, border, border-top 3px 등급 색상 (mezz-card 패턴)

**시나리오 테이블**:
컬럼: 시나리오 | 조건 | 현실성
- 현실성 컬럼: 색상 뱃지 (`비현실적` = red, `조건부` = yellow, `가능` = green)

**런웨이 테이블**:
컬럼: 투입금액 | 현 burn 기준 | 시나리오 B 기준
- 선순위 20억 경고 문구: `border: 1px solid --c-red` 컨테이너에 별도 표시

---

### H. KHK Decision

**3컬럼 레이아웃** (GO | NO-GO | 보류):

```
┌──────────────┬──────────────┬──────────────┐
│  GO 시나리오 │ NO-GO 시나리 │  보류 시나리  │
│  [GREEN]     │  [RED]       │  [YELLOW]     │
│              │              │               │
│  1. CB/RCPS  │  1. 금융권   │  1. 기술력    │
│     출자전환 │     부채      │     인정 후   │
│  2. 셔터스톡 │  2. 실매출   │     관찰      │
│  ...         │  ...         │               │
└──────────────┴──────────────┴──────────────┘
```

**스타일**:
- 각 컬럼: `crit-card` 패턴, border-top 3px 등급 색상
- 컬럼 헤더: Mono 9px uppercase 등급 색상
- 항목: Sans 12px 400 --text-sec, 번호 리스트
- KHK 직접 인용 (패턴 매칭 문구): Mono 11px --text-dim italic, `border-left: 2px solid --border-mid; padding-left: 8px`

---

### I. Next Actions

**우선순위 테이블**:
컬럼: 순위 | 액션 | 담당 | 기한

**스타일**:
- 1순위 행: bold, left border 3px --text-pri
- 담당 컬럼: Mono 10px 뱃지 (KYI = surface, KHK = yellow tint)
- 기한 컬럼: `즉시` = --c-red bold Mono

**분기 다이어그램**:
- ASCII 텍스트를 `<pre>` 태그 + Mono 11px --text-sec로 표시
- 배경: --surface, border: 1px solid --border, padding: 16px, border-radius: --r

---

### J. Email Draft

**기본 상태**: 접힌(collapsed) 블록

**헤더**:
```
[▶] 이메일 초안 — 사전 확인 요청   [복사 ↗]
```
- 헤더: Sans 12px 600 --text-sec
- 클릭 시 토글 (JS)

**펼쳐진 상태**:
- 배경: --surface, border: 1px solid --border, border-radius: --r
- 이메일 본문: Sans 12px 400 --text-sec, `white-space: pre-wrap; line-height: 1.8`
- 번호 항목들: 들여쓰기 16px
- 상단 우측 `[복사]` 버튼: Mono 9px, `navigator.clipboard.writeText` 동작

---

## 4. 상태 표시 시스템

### 4-1. Verdict (Overall)

| 값 | 배경 | 좌보더 | 도트 | 텍스트 |
|----|------|--------|------|--------|
| RED | `--verdict-red-bg` | `--c-red` | `--c-red` | `--c-red` |
| YELLOW | `--verdict-yellow-bg` | `--c-yellow` | `--c-yellow` | `--c-yellow` |
| GREEN | `--verdict-green-bg` | `--c-green` | `--c-green` | `--c-green` |

### 4-2. Gate 상태

| 아이콘 | 의미 | 보더색 | 뱃지 bg | 뱃지 fg |
|-------|------|--------|---------|---------|
| ❌ | FAIL | `--c-red` | `--bg-red` | `--c-red` |
| ⚠️ | WARN | `--c-yellow` | `--bg-yellow` | `--c-yellow` |
| ✅ | PASS | `--c-green` | `--bg-green` | `--c-green` |
| ⬜ | TBD | `--border-mid` | `--bg-unknown` | `--c-unknown` |

### 4-3. Flag 심각도

| 등급 | 좌보더 | 배경 | 뱃지 |
|------|--------|------|------|
| CRITICAL | `--c-red` | `--bg-red` (10% opacity 권장) | red |
| HIGH | `--c-orange` | `--bg-orange` (10% opacity) | orange |
| MEDIUM | `--c-yellow` | `--bg-yellow` (10% opacity) | yellow |

실제 적용: `--bg-red = #fee2e2`는 이미 연하므로 그대로 사용 가능.

### 4-4. 수치 색상 규칙

| 조건 | 색상 | 클래스 제안 |
|------|------|------------|
| 음수 수치 | `--c-red` | `.num-neg` |
| 양수 수치 | `--c-green` | `.num-pos` |
| 이상 수치 (급여 0.04억 등) | `--c-red` + bg `--bg-red` | `.num-alert` |
| 중립/참고 수치 | `--text-pri` | (기본) |
| 백분율 > 90% (보조금 97.8%) | `--c-red` bold | `.num-critical-pct` |

---

## 5. 테이블 스타일 규칙

### 5-1. 기본 테이블 (`.fin-table`)

```
width: 100%
border-collapse: collapse
font-size: 12px
```

| 요소 | 스타일 |
|------|--------|
| `thead th` | Mono 9px uppercase --text-dim, `padding: 6px 10px`, `border-bottom: 2px solid --border-mid`, `text-align: right` (수치 컬럼), `text-align: left` (항목 컬럼) |
| `tbody td` | `padding: 7px 10px`, `border-bottom: 1px solid --border`, Sans 12px |
| 항목명 `td:first-child` | `color: --text-sec`, `font-weight: 500` |
| 수치 셀 | Mono 12px `text-align: right` |
| 행 hover | `background: --surface` |
| 그룹 첫 행 | `border-top: 2px solid --border-mid` |
| 합계/소계 행 | `font-weight: 600`, `background: --surface` |
| 강조 행 | bg `--bg-red / --bg-yellow` |

### 5-2. 컴팩트 테이블 (`.fin-table.compact`)

- `font-size: 11px`
- `padding: 5px 8px`
- 인원 추이 테이블 등 행이 많은 경우

### 5-3. KPI 카드 내 수치

- 메인 수치: Mono 18~22px 600 (상황별)
- 보조 수치/비교: Sans 11px --text-dim
- 단위: Sans 11px --text-sec (수치 뒤 inline)

---

## 6. 반복 패턴

### 6-1. Section Block (`.dash-section`)

```
padding: 24px 24px 0
+ section-divider (height: 6px, bg: --surface2) 뒤에 붙음
```

내부 구성:
```
.section-label    — Mono 8px uppercase --text-dim letter-spacing: 0.18em
.section-title    — Sans 13px 700 --text-pri, margin-bottom: 4px
.section-sub      — Sans 12px --text-sec, margin-bottom: 16px, padding-bottom: 12px, border-bottom: 1px solid --border
[컨텐츠]
```

### 6-2. Info Pill (`.co-stat-pill` — 기존 재사용)

```
font-family: mono, 10px, --text-sec
background: --surface, border: 1px solid --border
padding: 3px 9px, border-radius: 3px
strong: --text-pri 600
```

변형:
- `.stat-pill-alert`: border `--c-red`, color `--c-red` (연체이력 등)
- `.stat-pill-warn`: border `#fde68a`, color `--c-yellow`

### 6-3. Status Badge (`.status-badge`)

```
font-family: mono, 8px 500 uppercase
padding: 2px 7px, border-radius: 3px
letter-spacing: 0.08em
```

색상은 4-1~4-3 매핑 적용.

### 6-4. Number Emphasis (`.num-kpi`)

대형 KPI 수치용:
```
font-family: mono, 20px 600
line-height: 1.2
```

서브텍스트 (비교/변화율):
```
font-size: 11px, font-family: sans, --text-dim
```

### 6-5. Divider

두 가지 구분 방식:
1. **섹션 간 구분**: `height: 6px; background: --surface2` (시각적 단절)
2. **섹션 내 구분**: `border-top: 1px solid --border` (연속성 유지)

### 6-6. Collapsible Block (`.collapsible`)

이메일 초안 등 선택적 열람 콘텐츠:
```
.collapsible-header: cursor pointer, display flex, justify-content space-between
  → hover: background --surface
.collapsible-body: display none (기본) / display block (open 시)
.collapsible-toggle: Mono 9px --text-dim, 회전 애니메이션 (▶ → ▼)
```

---

## 7. Sidebar 변경 사항 (최소한)

기존 `.company-item` 구조는 유지하되 하단 메타 정보 보완:

**기존**: 회사명 + 업종 + 날짜/시간 + verdict 뱃지

**v3 추가**:
- Stage 뱃지 (`.ci-stage`): 기존 클래스 그대로 (`s1/s2/s3`)
- 하단 메타: `설립 · Series A` 등 간략 투자 단계

**변경 없음**: 레이아웃, 너비(280px), 색상, 필터/검색 동작

---

## 8. 데이터 바인딩 참조 (FE용 JSON 키 힌트)

BE가 MD를 파싱하여 생성할 JSON 구조에서 각 섹션이 참조할 키:

```json
{
  "meta": {
    "company": "string",
    "ceo": "string",
    "founded": "YYYY-MM-DD",
    "description": "string",
    "cf_url": "string",
    "investment_total": "string",
    "investment_rounds": "number",
    "last_investment": "string",
    "gowid_limit": "string",
    "gowid_risk_grade": "number",
    "gowid_overdue_count": "number",
    "stage": "number"
  },
  "verdict": {
    "level": "RED|YELLOW|GREEN",
    "summary": "string",
    "detail": "string"
  },
  "gates": [
    {
      "id": "G1",
      "name": "string",
      "status": "FAIL|WARN|PASS|TBD",
      "finding": "string"
    }
  ],
  "findings": [
    { "no": 1, "text": "string", "severity": "CRITICAL|HIGH|null" }
  ],
  "financials": {
    "bs": { "years": [], "rows": [] },
    "cash_balance": { "months": [], "values": [], "mom_changes": [] },
    "dw_cf": { "months": [], "rows": [] },
    "cf_detail": { "in_rows": [], "out_rows": [] },
    "headcount": { "months": [], "rows": [] }
  },
  "ir_vs_fact": { "rows": [] },
  "red_flags": [
    {
      "severity": "CRITICAL|HIGH|MEDIUM",
      "no": 1,
      "title": "string",
      "body": "string"
    }
  ],
  "bep": {
    "current": {
      "monthly_revenue": "string",
      "monthly_burn": "string",
      "cash": "string",
      "runway": "string"
    },
    "scenarios": [],
    "runway_table": []
  },
  "khk_decision": {
    "go": [],
    "nogo": [],
    "hold": []
  },
  "next_actions": { "table": [], "flowchart": "string" },
  "email_draft": "string"
}
```

---

## 9. 구현 시 참조 파일

| 파일 | 참조 목적 |
|------|----------|
| `pitstop-portal/v4/index.html` | 전체 CSS 시스템, 레이아웃, 컴포넌트 클래스 |
| `[GONEX]/피트스탑/리콘랩스/피트스탑_준비_리콘랩스.md` | 실제 데이터 구조 및 표현 방식 레퍼런스 |
| 기존 `.crit-card`, `.mezz-card`, `.stage-bar`, `.co-stat-pill` | 컴포넌트 재사용 1순위 |
| 기존 `.grade-badge`, `.ci-verdict` | 상태 뱃지 재사용 |

---

*End of Design Spec v3 — 2026-04-06*
