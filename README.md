# DELIVERY SCHEDULE — 운영 가이드

## 폴더 구조

```
delivery-app/
├── index.html              ← 메인 앱 (수정 불필요)
├── netlify.toml            ← Netlify 배포 설정
├── data/
│   ├── products.json       ← 상품 데이터 (시즌마다 업데이트)
│   ├── delivery_schedule.json ← 딜리버리 날짜 (시즌마다 업데이트)
│   └── template_2026SS.csv ← 엑셀 작업용 CSV 템플릿
└── public/
    └── images/             ← 상품 이미지 (id와 파일명 일치)
```

---

## 시즌 업데이트 방법

### 1. 상품 추가 (products.json)

새 상품을 배열에 추가합니다.

```json
{
  "id": "TPES-XX00",
  "name": "상품명",
  "color": ["BLACK","WHITE"],
  "size": ["S","M","L"],
  "price": "00,000",
  "quality": "BODY/ COTTON 100%",
  "origin": "KOREA",
  "delivery_month": "2026-05",
  "notices": "",
  "image": "TPES-XX00.jpg"
}
```

**주의:** id와 image 파일명을 반드시 일치시켜야 합니다.

---

### 2. 딜리버리 날짜 수정 (delivery_schedule.json)

토요일 날짜를 직접 입력합니다.

```json
{
  "key": "2026-05",
  "label": "MAY",
  "saturdays": ["2026-05-02","2026-05-09","2026-05-16","2026-05-23","2026-05-30"]
}
```

---

### 3. 이미지 추가

`public/images/` 폴더에 파일을 넣습니다.
파일명 = id + `.jpg` (예: `TPES-SE02.jpg`)

---

### 4. CSV → JSON 변환 (클로드 코드 활용)

엑셀에서 CSV로 저장 후 클로드 코드에 붙여넣고 아래 지시:

```
이 CSV를 products.json 형식으로 변환해줘.
color와 size는 / 구분자로 나뉜 값을 배열로 바꿔줘.
```

---

## 배포 (Netlify)

1. 폴더 전체를 Netlify에 드래그앤드롭
2. 또는 GitHub 연동 후 자동 배포

---

## 버전 관리 (수동)

수정 전에 반드시 복사본 저장:

```
products_2026SS_backup_0515.json
```

---

## 파일 크기 기준

| 상품 수 | products.json 예상 크기 | 문제 여부 |
|--------|----------------------|--------|
| 100개  | ~30KB               | 없음    |
| 300개  | ~90KB               | 없음    |
| 500개  | ~150KB              | 없음    |
