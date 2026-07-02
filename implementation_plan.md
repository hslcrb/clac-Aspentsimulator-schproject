# 신규 기능 구현 계획

## 개요

세 가지 기능을 추가한다.

---

## 변경 내역

### 1. 버튼명 수정
#### [MODIFY] 배달...추정.html
- `한번에 100하기` → `한번에 100회 하기`

---

### 2. 모집단 데이터 편집 기능 (카드 내 인라인 편집)

기존 "표집 틀 설정" 카드의 연령대 체크박스 목록을 확장하여, 각 항목에
- **μ (평균)**, **σ (표준편차)**, **비중(weight %)** 을 직접 수정하는 `<input>` 필드 추가
- 상단에 **원본값으로 초기화** 버튼 (`resetPopulationData()`)
- `regionalData` 옆에 `originalRegionalData` (딥카피) 상수를 보관
- 입력값이 바뀔 때마다 `calculatePopulationMetrics()` → 차트 재계산

---

### 3. 지정 횟수 반복 추출 + 무한 모드

#### 추출 횟수 제어 UI (기존 배치 버튼 영역 교체)

```
┌──────────────────────────────────────┐
│  [표본 추출 1회]  [한번에 N회 하기]  │
│  추출 횟수:  [____100____] 회       │
│  ☑ 무한 반복 (∞)   → 정지 버튼     │
└──────────────────────────────────────┘
```

- **`batchCountInput`**: 숫자 입력 필드 (기본 100, 최소 1, 최대 99999)
- **`infiniteToggle`**: 체크박스 — ON이면 횟수 입력 비활성화
- **`한번에 N회 하기` 버튼**: `drawBatch()` 호출  
  - 무한 모드 OFF → `drawMultipleSamples(N)` 1회 실행
  - 무한 모드 ON → `setInterval`로 N회씩 계속 반복. 버튼 텍스트 "■ 정지"로 변환
- **`infiniteRunning`** 전역 플래그로 실행 상태 관리
- 자동 연속 추출(autoDrawToggle)과는 별도 인터벌로 관리

#### JS 추가 함수
```javascript
let batchCount = 100;
let infiniteIntervalId = null;
let infiniteRunning = false;

function updateBatchCount(val) { ... }      // 입력값 파싱 및 유효성 검사
function toggleInfiniteMode(checked) { ... } // 무한 모드 토글
function drawBatch() { ... }                 // 배치 실행 / 정지 통합 진입점
function stopInfinite() { ... }              // 명시적 정지
```

---

## Verification Plan
- HTML을 열어 μ/σ/weight 입력 후 참값 평균 카드가 즉시 업데이트되는지 확인
- 초기화 버튼 클릭 시 원본값 복원 확인
- 배치 횟수 50으로 설정 후 버튼 클릭 시 50회 추출 확인
- 무한 모드 ON → 버튼 "■ 정지"로 변환, 클릭 시 중단 확인
