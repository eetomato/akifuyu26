# 클로드 코드 실행 지시서 v3 (최종)
## DELIVERY SCHEDULE 웹앱 — 26FW 디자인 전면 리뉴얼

---

## 핵심 개념

Society 영수증 잡지 커버 레이아웃을 그대로 웹앱으로 구현한다.
기사 목록 → 월별 딜리버리 목록으로 1:1 매핑.

---

## 헤더 구조 (상단부터 순서대로)

```
QUINZOMADAIRE LIBRE ET INDÉPENDANT
→ 1/2 MISTER HOLLYWOOD
   font-size: 10px, letter-spacing: 3px, color: #666, text-align: center

Society (거대한 로고)
→ 2026 F/W
   font-size: 64px (모바일 52px), font-weight: 700
   font-family: 'Courier Prime', monospace
   letter-spacing: 4px, text-align: center, color: #0A0A0A

L'HEURE DE PAYER L'ADDITION
→ DELIVERY SCHEDULE
   font-size: 18px, font-weight: 700
   letter-spacing: 4px, text-align: center

날짜 표기
→ SEASON : AUG 2026 — FEB 2027
   font-size: 10px, letter-spacing: 2px, color: #555, text-align: center

******* 구분선 (페이지 전체 너비 가득) *******
```

---

## 메인 콘텐츠 — 월 목록 (Society 기사 목록 구조)

Society 원본:
```
L'INCROYABLE DETTE          P.54
Comment la France va régler ses factures
```

웹앱 매핑:
```
AUGUST                      (12 ITEMS)
8/1  8/8  8/15  8/22  8/29
```

### 각 월 행 상세 스펙

```
월 이름 (AUGUST):
  font-size: 28px
  font-weight: 700
  letter-spacing: 3px
  color: #0A0A0A
  display: flex
  justify-content: space-between

수량 표기 (12 ITEMS):
  font-size: 14px
  font-weight: 700
  letter-spacing: 1px
  color: #0A0A0A
  align-self: flex-end

날짜 아이콘 행:
  font-size: 11px, letter-spacing: 1px, color: #555
  margin-top: 4px, display: flex, gap: 8px

날짜 아이콘 개별 버튼:
  font-family: 'Courier Prime', monospace
  font-size: 10px, letter-spacing: 1px
  padding: 3px 7px
  border: 1px solid #0A0A0A
  border-radius: 0
  background: transparent, cursor: pointer, color: #0A0A0A

  활성(클릭됨):
    background: #0A0A0A, color: #FAFAF7

월 행 하단:
  border-bottom: 1px solid #ddd
  padding-bottom: 14px, margin-bottom: 14px
```

### 전체 월 목록 순서 (26FW)

```
******* (구분선)
AUGUST              (X ITEMS)
SEPTEMBER           (X ITEMS)
OCTOBER             (X ITEMS)
NOVEMBER            (X ITEMS)
DECEMBER            (X ITEMS)
JANUARY             (X ITEMS)
FEBRUARY            (X ITEMS)
******* (구분선)
```

※ X ITEMS = products.json에서 해당 월 상품 수 자동 계산

---

## 날짜 클릭 시 동작

날짜 아이콘 클릭 → 해당 월 행 바로 아래 가로 스크롤 스트립 등장
(기존 JS 로직 유지, 위치만 변경)

---

## ★ 종이 질감 (반드시 구현) ★

레퍼런스 이미지에서 확인된 3가지 질감을 모두 CSS로 구현한다.
외부 이미지 파일 사용 금지 — 순수 CSS만으로 구현.

### 1. 구겨진 종이 질감 (수평 라인 노이즈)

```css
.page {
  background-color: #FAFAF7;
  background-image:
    repeating-linear-gradient(
      0deg,
      transparent,
      transparent 3px,
      rgba(0,0,0,0.018) 3px,
      rgba(0,0,0,0.018) 4px
    ),
    repeating-linear-gradient(
      90deg,
      transparent,
      transparent 40px,
      rgba(0,0,0,0.008) 40px,
      rgba(0,0,0,0.008) 41px
    );
}
```

### 2. 붉은 세로 번짐선 (열전사 잉크 번짐 효과)

페이지 우측에 붉은 세로선이 흐릿하게 번진 효과.
Society 이미지에서 가장 특징적인 요소.

```css
.page::after {
  content: '';
  position: absolute;
  top: 0;
  right: 18%;          /* 우측에서 약간 안쪽 */
  width: 3px;
  height: 100%;
  background: linear-gradient(
    to bottom,
    transparent 0%,
    rgba(180, 40, 40, 0.25) 8%,
    rgba(200, 50, 50, 0.45) 15%,
    rgba(180, 40, 40, 0.35) 25%,
    transparent 35%,
    rgba(160, 30, 30, 0.2) 45%,
    rgba(190, 45, 45, 0.4) 55%,
    rgba(170, 35, 35, 0.3) 65%,
    transparent 75%,
    rgba(150, 30, 30, 0.15) 85%,
    transparent 100%
  );
  pointer-events: none;
  z-index: 10;
}

/* 붉은선 옆 얇은 보조선 */
.page::before {
  content: '';
  position: absolute;
  top: 0;
  right: calc(18% + 5px);
  width: 1px;
  height: 100%;
  background: linear-gradient(
    to bottom,
    transparent 0%,
    rgba(180, 40, 40, 0.1) 10%,
    rgba(200, 50, 50, 0.2) 20%,
    transparent 30%,
    rgba(160, 30, 30, 0.15) 50%,
    rgba(190, 45, 45, 0.2) 60%,
    transparent 70%,
    transparent 100%
  );
  pointer-events: none;
  z-index: 10;
}

/* .page는 position: relative 필수 */
.page {
  position: relative;
  overflow: hidden;
}
```

### 3. 영수증 용지 끊김 효과 (상하단 톱니)

영수증 상단과 하단에 찢어진 용지 느낌.

```css
/* 상단 톱니 */
.page-top-tear {
  width: 100%;
  height: 12px;
  background-image: repeating-linear-gradient(
    90deg,
    #FAFAF7 0px,
    #FAFAF7 6px,
    transparent 6px,
    transparent 10px
  );
  margin-bottom: -2px;
}

/* 하단 톱니 */
.page-bottom-tear {
  width: 100%;
  height: 12px;
  background-image: repeating-linear-gradient(
    90deg,
    #FAFAF7 0px,
    #FAFAF7 6px,
    transparent 6px,
    transparent 10px
  );
  margin-top: -2px;
}
```

HTML 구조:
```html
<div class="page-top-tear"></div>
<div class="page">
  ... (기존 콘텐츠)
</div>
<div class="page-bottom-tear"></div>
```

---

## 전체 레이아웃

```
max-width: 520px
margin: 0 auto
background: #FAFAF7
padding: 24px 20px 80px

body:
  background: #D8D4CC   ← 어두운 회색 배경 (종이가 떠 있는 느낌)
  min-height: 100vh

box-shadow:
  4px 4px 16px rgba(0,0,0,0.25),
  -2px -2px 8px rgba(0,0,0,0.1)
```

---

## 상품 모달

```
모달 헤더:
  background: #0A0A0A, color: #FAFAF7, border-radius: 0

모달 본문:
  background: #FAFAF7
  border: 1.5px solid #0A0A0A
  border-radius: 0

행(row) key: color: #888, letter-spacing: 1.5px
행(row) value: color: #0A0A0A, font-weight: 700
```

---

## 구분선

```
굵은 구분선:
  * * * * * * * * * * * * * * * * * * * * * * * * * * *
  font-size: 11px, letter-spacing: 1px
  color: #222, text-align: center, padding: 10px 0

얇은 구분선:
  각 월 행 border-bottom: 1px solid #ddd
```

---

## 폰트

```html
<link href="https://fonts.googleapis.com/css2?family=Courier+Prime:wght@400;700&display=swap" rel="stylesheet">
```

---

## 데이터 구조 (delivery_schedule.json 교체)

```json
{
  "season": "2026 F/W",
  "period": "AUG 2026 — FEB 2027",
  "brand": "1/2 MISTER HOLLYWOOD",
  "months": [
    { "key": "2026-08", "label": "AUGUST",    "saturdays": ["2026-08-01","2026-08-08","2026-08-15","2026-08-22","2026-08-29"] },
    { "key": "2026-09", "label": "SEPTEMBER", "saturdays": ["2026-09-05","2026-09-12","2026-09-19","2026-09-26"] },
    { "key": "2026-10", "label": "OCTOBER",   "saturdays": ["2026-10-03","2026-10-10","2026-10-17","2026-10-24","2026-10-31"] },
    { "key": "2026-11", "label": "NOVEMBER",  "saturdays": ["2026-11-07","2026-11-14","2026-11-21","2026-11-28"] },
    { "key": "2026-12", "label": "DECEMBER",  "saturdays": ["2026-12-05","2026-12-12","2026-12-19","2026-12-26"] },
    { "key": "2027-01", "label": "JANUARY",   "saturdays": ["2027-01-03","2027-01-10","2027-01-17","2027-01-24","2027-01-31"] },
    { "key": "2027-02", "label": "FEBRUARY",  "saturdays": ["2027-02-07","2027-02-14","2027-02-21","2027-02-28"] }
  ]
}
```

---

## 제거 항목

- 상단 월 필터 버튼 행 (JAN FEB MAR... 버튼)
- ALL ITEMS 썸네일 그리드

---

## 유지 항목 (절대 변경 금지)

- fetch()로 JSON 로딩하는 방식
- 날짜 클릭 → 스크롤 스트립 동작
- 상품 클릭 → 모달 팝업 동작
- SCHEDULE TBD 배지 동작
- products.json / delivery_schedule.json 파일 구조

---

## 완료 후 확인사항

- [ ] 헤더: 1/2 MISTER HOLLYWOOD → 2026 F/W → DELIVERY SCHEDULE 순서
- [ ] 우측 붉은 세로 번짐선 표시
- [ ] 수평 라인 노이즈 질감 표시
- [ ] 상하단 톱니 찢김 효과 표시
- [ ] 월 목록 세로 배치 (Society 기사 목록 형식)
- [ ] 날짜 아이콘 클릭 → 해당 월 아래 스트립 등장
- [ ] 모달 헤더 검정 반전
- [ ] 모바일(375px) 레이아웃 이상 없음
- [ ] Netlify 배포 후 전체 동작 확인
