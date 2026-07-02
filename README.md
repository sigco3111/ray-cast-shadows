# 💡 Ray-Cast Shadows — Dynamic Light & Global Illumination

> 2D 캔버스에서 마우스 커서를 광원(Light Source)으로 삼아, 임의 배치된 미로 벽들 사이로 **실시간 동적 그림자(Ray Casting Shadow)** 를 드리우고, **클릭으로 광원 색상**을 바꾸며 **은은한 글로벌 일루미네이션** 느낌을 구현한 제너레이티브 아트.

![status](https://img.shields.io/badge/status-live-brightgreen) ![tech](https://img.shields.io/badge/tech-HTML5%20Canvas%20%2B%20Vanilla%20JS-blue) ![license](https://img.shields.io/badge/license-MIT-yellow)

---

## 🎬 라이브 데모 (Live Demo)

| 항목 | 값 |
|------|-----|
| **Live URL** | https://ray-cast-shadows.vercel.app |
| **상태** | 🟢 Production |
| **렌더링** | 단일 HTML, 외부 의존성 0 |
| **반응형** | viewport 자동 맞춤, 모바일 터치 지원 |

[![Live Demo](https://img.shields.io/badge/Vercel-Live%20Demo-black?style=for-the-badge&logo=vercel)](https://ray-cast-shadows.vercel.app)

**빠른 사용법**: 페이지 열기 → 마우스로 광원 이동 → 클릭으로 색상 변경 → 미로 사이로 빛이 퍼짐

---

## 🤖 생성 정보 (How this was made)

이 프로젝트는 다음 프롬프트를 **그대로** MiniMax-M3에 전달하여 생성되었습니다.

> 2D 캔버스 상에서 복잡한 미로 형태의 벽들이 임의로 배치되고, 마우스 커서가 광원(Light Source)이 되어 실시간으로 벽에 막히는 동적인 그림자(Ray Casting Shadow)를 생성하되, 광원의 색상을 클릭할 때마다 변경할 수 있고 빛이 벽에 반사되어 은은하게 퍼지는 글로벌 일루미네이션 느낌의 조명 효과를 구현해줘.
>
> **Implementation Advice:** Use HTML5 Canvas. Implement Ray Casting from the light source (mouse) to all wall endpoints. Sort angles, cast rays, and draw polygon fills for the light. For "global illumination" feel, use multiple light passes or cheat with radial gradients. 모든 의존관계의 코드를 하나의 HTML에 담는 형태로 코드 작성.

| 항목 | 값 |
|------|-----|
| AI 모델 | **MiniMax-M3** (via MiniMax API, MoA fallback chain) |
| 코딩 에이전트 | **[OpenCode](https://github.com/anomalyco/opencode)** v1.17.13 |
| 플랫폼 | macOS (Apple Silicon / M4) |
| 라이선스 | MIT |

---

## ✨ Features

- 💡 **동적 광원** — 마우스 커서 위치를 실시간 추적, 미로 벽 사이로 빛이 자연스럽게 퍼짐
- 🌑 **Ray Casting 그림자** — 모든 벽 endpoint까지 각도 정렬 + polygon fill로 정확하고 부드러운 동적 그림자
- 🎨 **클릭 = 색상 사이클** — 클릭할 때마다 미리 정의된 파스텔 팔레트(따뜻한 흰색 → 시원한 시안 → 노을 골드 → 민트 → 로즈) 순환
- 🌟 **글로벌 일루미네이션** — 1차 ray polygon + 2차 radial gradient overlay로 은은한 빛 번짐
- 🧱 **절차적 미로** — 매 새로고침마다 다른 다중 폴리곤 벽, 시드 가능한 난수
- ⚡ **60fps 부드러운 애니메이션** — requestAnimationFrame + 캔버스 더블 버퍼링
- 🖱️ **실시간 인터랙션** — mousemove 추적, click 색상 변경, viewport 반응형

---

## 🚀 사용법 (Usage)

### 라이브 데모
👉 https://ray-cast-shadows.vercel.app 에서 바로 실행

### 로컬 실행

```bash
# 1. 클론
git clone https://github.com/sigco3111/ray-cast-shadows.git
cd ray-cast-shadows

# 2. 브라우저로 열기
open index.html            # macOS
xdg-open index.html        # Linux

# 또는 로컬 서버
python3 -m http.server 8000
# → http://localhost:8000
```

### 인터랙션

| 동작 | 효과 |
|------|------|
| **마우스 이동** | 광원이 마우스 위치로 즉시 이동, 그림자 실시간 갱신 |
| **클릭** | 광원 색상이 다음 팔레트로 변경 (5종 순환) |
| **드래그** | 색상 사이클 가속 (연속 변경) |

---

## Tech Stack

| 항목 | 값 |
|------|-----|
| 렌더링 | HTML5 Canvas 2D Context |
| 언어 | Vanilla JavaScript (ES6+) |
| 스타일 | CSS3 (Custom Properties + Filter effects) |
| 빌드 | 없음 (zero-deps) |
| 호스팅 | Vercel (정적 사이트) |

### 알고리즘 요약

```js
// 의사 코드 — 실제 구현은 index.html 참조
function castRays(lightPos, walls) {
  // 1. 모든 벽 endpoint까지의 각도 계산
  const angles = walls.flatMap(w => [
    Math.atan2(w.p1.y - lightPos.y, w.p1.x - lightPos.x),
    Math.atan2(w.p2.y - lightPos.y, w.p2.x - lightPos.x)
  ]);
  
  // 2. 각도 정렬 → ray endpoints
  const rayPoints = angles.sort().map(a => ({
    x: lightPos.x + Math.cos(a) * MAX_DIST,
    y: lightPos.y + Math.sin(a) * MAX_DIST
  }));
  
  // 3. Polygon fill로 빛 영역 그림
  ctx.fillStyle = gradient;
  ctx.beginPath();
  ctx.moveTo(lightPos.x, lightPos.y);
  rayPoints.forEach(p => ctx.lineTo(p.x, p.y));
  ctx.closePath();
  ctx.fill();
  
  // 4. 글로벌 일루미네이션 — radial gradient overlay
  const rg = ctx.createRadialGradient(lightPos.x, lightPos.y, 0, lightPos.x, lightPos.y, MAX_DIST);
  rg.addColorStop(0, lightColor + 'AA');
  rg.addColorStop(1, lightColor + '00');
  ctx.fillStyle = rg;
  ctx.fillRect(0, 0, w, h);
}
```

---

## 🗺️ 로드맵

- [x] **v0.1** — 기본 Ray Casting (단일 광원 + 미로 + polygon fill)
- [x] **v0.2** — 마우스 추적 + 글로벌 일루미네이션 (radial gradient)
- [x] **v0.3** — 클릭 = 색상 사이클 (5종 팔레트)
- [x] **v0.4** — 벽 굴절 / 부드러운 빛 가장자리 (Multi-Light + God Rays)
- [x] **v0.5** — HUD + 키보드 컨트롤 (팔레트 직접 선택 + 미로 리롤)
- [ ] **v0.6** — 사운드 (빛 이동 ambient + 클릭 시미언트)
- [ ] **v0.7** — 멀티 라이트 모드 (여러 광원 동시)

---

## 🇺🇸 English

> A **mouse-driven light source** casting **dynamic ray-casting shadows** through procedurally-generated maze walls, with **click-to-cycle colors** and a soft **global illumination** glow — generative art in HTML5 Canvas.

- **Live Demo**: https://ray-cast-shadows.vercel.app
- **Algorithm**: Ray casting (angles to wall endpoints) + polygon fill + radial gradient overlay
- **Tech**: HTML5 Canvas, Vanilla JS, zero dependencies
- **License**: MIT

---

## 📝 라이선스

MIT License — 자유롭게 사용, 수정, 배포 가능

---

## 🙏 Credits

- **Ray Casting 알고리즘** — 고전 2D 광선 추적 기법
- **글로벌 일루미네이션** — 다중 패스 + radial gradient 근사
- **미로 절차적 생성** — 시드 가능한 난수 + 다중 폴리곤
- **OpenCode + MiniMax-M3** — AI 협업 부트스트랩
- **Vercel** — 정적 사이트 호스팅
- 코딩미션 참조 페이지: [cokac.com](https://cokac.com/list/announcement/24)

---

<p align="center"><sub>💡 Built with OpenCode + MiniMax-M3 · sigco3111 · MIT · AI-generated</sub></p>