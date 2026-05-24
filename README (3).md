# 화장품 O/W·W/O 에멀젼 호모믹서 스케일업 시뮬레이터 (V14)

화장품 제조 공정의 호모믹서(Homo-mixer)를 3D로 시각화하고, 5단계 전공정(수상 분산 → 유상 분산 → 유화 → 1차 냉각·재분산 → 2차 냉각·QC)을 시뮬레이션하는 웹 기반 도구입니다.

## 기능 개요

- **3D 시각화**: Three.js 기반 가마(메인 + 용해조) + 호모/패들/스크래퍼 임펠러 + 유체 입자 렌더링
- **5단계 자동 시퀀스** 또는 수동 RPM 조정 모드
- **3종 가마 스케일업** (100L / 300L / 500L) — 등-ε 권장 RPM 계산
- **물리 모델**
  - Rushton 식 기반 에너지 소산율 ε 계산
  - Calabrese 모델 기반 D₃₂ 평형 / 시간 진화
  - **PBM (Population Balance Model)** — Calvo et al.(2024), Fixed-Pivot (Kumar & Ramkrishna 1996)
  - Shannon Entropy 기반 분포 균일도 (H-Index)
  - 화장품 실측 범위에 맞춘 점도/경도 모델 (V14에서 재설계)
- **출하 QC 판정**: 사용자 정의 점도(cps) / 경도(mm) 스펙 + D₃₂ + H-Index 동시 검증
- **데이터 출력**: CSV 저장, 화면 캡처, 녹화

## 사용 방법

별도의 빌드 과정이 필요 없습니다. 정적 파일 1개만 있으면 됩니다.

### 1. 직접 열기
브라우저에서 `index.html`을 더블클릭해도 동작합니다.
단, Three.js를 CDN(jsdelivr)에서 ES Module로 가져오므로 **인터넷 연결이 필요**합니다.

### 2. 로컬 서버로 띄우기 (권장)
일부 브라우저는 `file://` 프로토콜에서 importmap을 제한할 수 있습니다. 간단히:

```bash
# Python 3
python -m http.server 8000

# 또는 Node.js
npx serve .
```

이후 `http://localhost:8000` 접속.

### 3. GitHub Pages 배포
저장소 Settings → Pages → Source를 `main` 브랜치 `/ (root)`로 지정하면 바로 호스팅됩니다.

## 기술 스택

- **Three.js 0.160.0** (CDN, ES Module + importmap)
- **OrbitControls** (Three.js addons)
- 순수 JavaScript / CSS / HTML — 빌드 도구 없음
- 차트는 `<canvas>` 2D 컨텍스트로 직접 구현 (MiniChart 클래스)

## 파일 구조

```
emulsion-simulator/
├── index.html      # 전체 시뮬레이터 (HTML + CSS + JS 단일 파일)
└── README.md
```

`index.html` 한 파일 안에 다음이 모두 들어있습니다:
- `<style>` (라인 6~718): 다크 테마 UI, 3패널 그리드 레이아웃
- `<body>` (라인 720~1253): 좌측 패널(설비/배합/환경) · 중앙 캔버스 · 우측 패널(RPM/QC) · 하단 차트 4개
- `<script type="module">` (라인 1264~3968): Physics 모듈, Vessel3D / Dissolver3D 클래스, simState, tick 루프

## 주요 모델 출처

- **Calabrese et al.** — D₃₂ 평형식 (Weber number 기반)
- **Coulaloglou & Tavlarides** — 이론값 비교용
- **Calvo et al. (2024)** *"A population balance model for cosmetic emulsion design"*, Chem. Eng. Sci. 287:119737
- **Kumar & Ramkrishna (1996)** — Fixed-Pivot 이산화
- **Pal-Rhodes** — 묽은 에멀젼 점도 (V14에서 화장품 범위에 맞게 곱셈 모델로 재설계)

## 라이선스

원작자 표기 후 자유 사용. 학습/연구/사내 데모 목적 권장.
