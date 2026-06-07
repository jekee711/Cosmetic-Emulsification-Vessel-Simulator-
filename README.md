# 화장품 O/W·W/O 에멀젼 호모믹서 스케일업 시뮬레이터 (V15)

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

## 파일 구조

```
.
├── index.html      # 마크업 + Three.js importmap + 외부 파일 링크
├── style.css       # 다크 테마 UI, 3패널 그리드 레이아웃
├── script.js       # Physics 모듈, Vessel3D / Dissolver3D 클래스, simState, tick 루프
├── README.md
├── LICENSE
└── .gitignore
```

## 실행 방법

### 1. GitHub Pages (가장 간단)
1. 저장소의 **Settings → Pages**로 이동
2. **Source**: `Deploy from a branch`
3. **Branch**: `main` / `/ (root)` 선택 후 **Save**
4. 1~2분 후 `https://<사용자명>.github.io/<저장소명>/` 접속

### 2. 로컬 서버 (개발용)
브라우저가 `file://`에서 ES Module importmap을 막을 수 있으므로 로컬 서버를 띄우는 것을 권장합니다.

```bash
# Python 3 (별도 설치 불필요)
python -m http.server 8000

# 또는 Node.js
npx serve .
```

이후 브라우저에서 `http://localhost:8000` 접속.

> Three.js는 jsdelivr CDN에서 로드하므로 **인터넷 연결이 필요**합니다.

## 기술 스택

- **Three.js 0.160.0** (CDN, ES Module + importmap)
- **OrbitControls** (Three.js addons)
- 순수 JavaScript / CSS / HTML — 빌드 도구 없음
- 차트는 `<canvas>` 2D 컨텍스트로 직접 구현 (MiniChart 클래스)

## 주요 모델 출처

- **Calabrese et al.** — D₃₂ 평형식 (Weber number 기반)
- **Coulaloglou & Tavlarides** — 이론값 비교용
- **Calvo et al. (2024)** *"A population balance model for cosmetic emulsion design"*, Chem. Eng. Sci. 287:119737
- **Kumar & Ramkrishna (1996)** — Fixed-Pivot 이산화
- **Pal-Rhodes** — 묽은 에멀젼 점도 (V14에서 화장품 범위에 맞게 곱셈 모델로 재설계)

## 라이선스

MIT License. 자세한 내용은 `LICENSE` 파일 참조.
