# 💡 Ray-Cast Shadows — Dynamic Light & Global Illumination

> 2D 캔버스에서 마우스 커서를 광원(Light Source)으로 삼아, 임의 배치된 미로 벽들 사이로 **실시간 동적 그림자(Ray Casting Shadow)** 를 드리우고, **클릭으로 광원 색상**을 바꾸며 **은은한 글로벌 일루미네이션** 느낌을 구현한 제너레이티브 아트.

단일 HTML 파일에 모든 의존관계를 담아, 별도 빌드 없이 브라우저에서 바로 실행됩니다.

---

## ✨ 결정 사항

| # | 결정 | 이유 |
|---|------|------|
| 1 | 단일 HTML5 Canvas | 외부 의존성 0, 더블클릭으로 실행 |
| 2 | Ray Casting (광선 투사) | 마우스 위치를 광원으로, 모든 벽 endpoint까지 ray 각도 정렬 후 polygon fill |
| 3 | 미로 벽은 절차적 생성 | 시드 가능한 난수 + 다중 폴리곤 (미리 정의된 maze X, 매번 새로) |
| 4 | 글로벌 일루미네이션 = 다중 패스 | 1차 ray casting + 2차 radial gradient overlay (은은한 빛 번짐) |
| 5 | 클릭 = 색상 사이클 | 미리 정의된 5~6개 파스텔 팔레트 중 다음으로 토글 |
| 6 | 실시간 60fps | requestAnimationFrame + 캔버스 더블 버퍼링 |

---

## 🛠️ 제작 환경

이 저장소의 초기 README와 프로젝트 부트스트랩은 다음 환경에서 진행되었습니다.

| 항목 | 값 |
|------|-----|
| AI 모델 | **MiniMax-M3** (via MiniMax API, MoA fallback chain) |
| 코딩 에이전트 | **[OpenCode](https://github.com/anomalyco/opencode)** v1.17.13 |
| 플랫폼 | macOS (Apple Silicon / M4) |
| 라이선스 | MIT |

> 핵심 알고리즘(`index.html`의 `<script>` 블록)은 위 환경의 **MiniMax-M3** 모델이 OpenCode CLI를 통해 작성했습니다.

---

## 📜 원본 미션 프롬프트

이 프로젝트는 아래 프롬프트를 **그대로** MiniMax-M3에 전달하여 생성되었습니다.

> 2D 캔버스 상에서 복잡한 미로 형태의 벽들이 임의로 배치되고, 마우스 커서가 광원(Light Source)이 되어 실시간으로 벽에 막히는 동적인 그림자(Ray Casting Shadow)를 생성하되, 광원의 색상을 클릭할 때마다 변경할 수 있고 빛이 벽에 반사되어 은은하게 퍼지는 글로벌 일루미네이션 느낌의 조명 효과를 구현해줘.
>
> **Implementation Advice:** Use HTML5 Canvas. Implement Ray Casting from the light source (mouse) to all wall endpoints. Sort angles, cast rays, and draw polygon fills for the light. For "global illumination" feel, use multiple light passes or cheat with radial gradients. 모든 의존관계의 코드를 하나의 HTML에 담는 형태로 코드 작성.

요구사항 체크리스트:

- [ ] 2D 캔버스 + 임의 배치된 미로 벽
- [ ] 마우스 커서 = 광원 (실시간 추적)
- [ ] Ray Casting 그림자 (각도 정렬 + polygon fill)
- [ ] 클릭 = 광원 색상 변경
- [ ] 글로벌 일루미네이션 느낌 (다중 패스 / radial gradient)
- [ ] HTML5 Canvas + 단일 HTML (의존성 0)

---

## 📁 구조

```
ray-cast-shadows/
├── index.html        ← 전체 앱 (HTML + CSS + Canvas + JS)
└── README.md
```

별도 의존성 없음. `index.html`을 브라우저로 열면 바로 실행됩니다.

---

## ▶️ 실행

```bash
# 옵션 1: 브라우저에서 직접 열기
open index.html

# 옵션 2: 로컬 서버 (권장)
python3 -m http.server 8000
# → http://localhost:8000
```

---

## 🗺️ 로드맵

- [ ] **v0.1** — 기본 Ray Casting (단일 광원 + 미로 + polygon fill)
- [ ] **v0.2** — 마우스 추적 + 글로벌 일루미네이션 (radial gradient)
- [ ] **v0.3** — 클릭 = 색상 사이클 (5~6개 팔레트)
- [ ] **v0.4** — 벽 굴절 / 부드러운 빛 가장자리
- [ ] **v0.5** — 키보드 컨트롤 (팔레트 직접 선택 + 미로 리롤)
- [ ] **v0.6** — 사운드 (빛 이동 ambient + 클릭 시미언트)

---

## 📜 라이선스

MIT