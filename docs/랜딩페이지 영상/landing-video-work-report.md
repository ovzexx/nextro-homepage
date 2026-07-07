# NEXTRO 랜딩페이지 영상 작업 인수인계 보고서

최종 갱신: 2026-07-07 21:03 KST

## 위치

- 로컬 프로젝트: `/Users/topjh/Desktop/넥스트로 `
- 메인 파일: `index.html`
- 영상 파일 폴더: `assets/`
- 배포 URL: `https://nextro-homepage.vercel.app/index.html`
- GitHub: `https://github.com/ovzexx/nextro-homepage`
- 현재 브랜치: `main`

주의: 프로젝트 폴더명 끝에 공백이 있다. 터미널에서 경로를 다룰 때 따옴표로 감싸는 것이 안전하다.

## 현재 Git 상태

확인 시점 기준:

- `HEAD`: `241c1ba feat: 모바일 반응형 CSS 추가`
- `origin/main`: `241c1ba`
- 이전 영상 관련 커밋도 히스토리에 남아 있음:
  - `f67e0fd fix: remove distracting slide 4 clips`
  - `9501b9d fix: replace slide 4 innovation videos`

현재 로컬에는 뉴스 이미지 관련 미커밋 변경이 남아 있을 수 있다. 영상 작업만 이어갈 때는 `git status -sb`로 먼저 확인하고, 관련 없는 변경은 커밋에 섞지 말 것.

## 구현 방식

히어로 슬라이드는 각 슬라이드마다 `.hero-video-scene` 안에 2개의 `<video>` 태그를 둔다.

JavaScript의 `setupHeroVideoMontage(sceneSelector, sources, intervalMs)`가 두 비디오를 번갈아 활성화하면서 여러 영상 소스를 순환한다.

핵심 함수:

- `playHeroScene(video, src)`
- `setupHeroVideoMontage(sceneSelector, sources, intervalMs)`

현재 호출:

```js
setupHeroVideoMontage('.hero-industrial-video-scene', industrialSceneSources, 2400);
setupHeroVideoMontage('.hero-ai-video-scene', aiSceneSources, 2800);
setupHeroVideoMontage('.hero-smart-factory-video-scene', smartFactorySceneSources, 2400);
setupHeroVideoMontage('.hero-innovation-video-scene', innovationSceneSources, 2600);
```

## 현재 슬라이드별 영상 소스

### 1슬라이드: AI Solution

의도: AI 기술 기업 첫 화면. 서버, 데이터, 코드, AI 솔루션 화면 중심.

```js
const industrialSceneSources = [
  'assets/nextro-hero-digital-network.mp4',
  'assets/nextro-hero-server-infrastructure.mp4',
  'assets/nextro-hero-code-screen.mp4',
  'assets/nextro-ai-bigdata.mp4',
  'assets/nextro-ai-pdm.mp4',
  'assets/nextro-ai-solution.mp4'
];
```

주의:

- 자전거/수리처럼 보인 `nextro-hero-engineer-tablet-server.mp4`는 쓰지 않는다.
- `nextro-hero-digital-tablet.mp4`, `nextro-hero-ai-engineering-work.mp4`도 1슬라이드에서 제외했다.
- 화력발전소, 용광로, 석탄, 중공업 느낌 영상도 1슬라이드에서 제외했다.

### 2슬라이드: Predictive Maintenance

의도: 예지보전, AI 분석, 데이터센터, 관제.

```js
const aiSceneSources = [
  'assets/nextro-ai-datacenter-engineers.mp4',
  'assets/nextro-ai-control-room.mp4',
  'assets/nextro-ai-data-center-hallway.mp4',
  'assets/nextro-ai-data-visualization.mp4'
];
```

### 3슬라이드: Smart Factory

의도: 로봇팔, 자동화 생산라인, 회로 조립. 사용자가 가장 만족한 방향.

```js
const smartFactorySceneSources = [
  'assets/nextro-scene-robot-production-line.mp4',
  'assets/nextro-scene-electronics-robot.mp4',
  'assets/nextro-scene-automated-circuit-placement.mp4',
  'assets/nextro-scene-robotic-arm.mp4',
  'assets/nextro-scene-electronics-assembly-line.mp4',
  'assets/nextro-scene-circuit-board-line.mp4'
];
```

### 4슬라이드: Innovation

의도: 기괴한 AI 추상 이미지가 아니라 기업/도시/인프라 중심의 차분한 혁신 이미지.

```js
const innovationSceneSources = [
  'assets/nextro-innovation-corporate-buildings.mp4',
  'assets/nextro-innovation-city-train.mp4',
  'assets/nextro-innovation-city-night-grid.mp4',
  'assets/nextro-innovation-city-aerial-night.mp4'
];
```

중요:

- 두뇌 이미지가 전환 순간에 희미하게 보였던 원인은 영상 파일이 아니라 `.hero-innovation-video-scene::before` fallback 이미지였다.
- 현재 fallback은 반드시 아래처럼 유지해야 한다.

```css
.hero-innovation-video-scene::before {
  background-image: none;
  background-color: #05080b;
  background-position: center center;
  filter: none;
}
```

절대 다시 넣지 말 것:

- `https://images.unsplash.com/photo-1677442135703-1787eea5ce01...`
- `assets/nextro-innovation-business-meeting.mp4`
- `assets/nextro-innovation-business-graphs.mp4`
- `assets/nextro-innovation-digital-screens.mp4`
- `assets/nextro-innovation-matrix-data.mp4`
- `assets/nextro-innovation-cloud-data.mp4`
- `assets/nextro-innovation-processor-circuit.mp4`
- `assets/nextro-innovation-digital-tunnel.mp4`
- `assets/nextro-innovation-coding-tech.mp4`

## .gitignore 관련 주의

`.gitignore`에 `*.mp4`가 들어 있었거나 들어 있을 수 있다.

새 영상 파일을 GitHub/Vercel에 올리려면 일반 `git add`로는 추가되지 않을 수 있으므로 필요 시 강제 추가한다.

```bash
git add index.html
git add -f assets/새영상파일.mp4
git commit -m "fix: update landing videos"
git push origin main
```

## Vercel 배포 방식

GitHub 푸시 후 Vercel 자동 배포가 안 되거나 오래 걸리면 Vercel CLI로 직접 배포했다.

프로젝트는 `.vercel/project.json`에 연결 정보가 있다.

```bash
vercel --prod --yes --scope team_s4pPnoH1gLBWZ21AZgFrnVRl
```

배포 후 확인:

```bash
curl -sL "https://nextro-homepage.vercel.app/index.html?check=$(date +%s)" \
  | rg "nextro-innovation|hero-innovation-video-scene|1677442135703"

curl -I https://nextro-homepage.vercel.app/assets/next로-확인할-영상.mp4
```

## 검증 명령

### inline script 문법 확인

```bash
node - <<'NODE'
const fs = require('fs');
const html = fs.readFileSync('index.html', 'utf8');
[...html.matchAll(/<script[^>]*>([\s\S]*?)<\/script>/gi)]
  .map(m => m[1])
  .filter(Boolean)
  .forEach(s => new Function(s));
console.log('inline script ok');
NODE
```

### 현재 슬라이드 영상 배열 확인

```bash
node - <<'NODE'
const fs = require('fs');
const html = fs.readFileSync('index.html', 'utf8');
for (const name of [
  'industrialSceneSources',
  'aiSceneSources',
  'smartFactorySceneSources',
  'innovationSceneSources'
]) {
  const match = html.match(new RegExp(`const ${name} = \\\\[([\\\\s\\\\S]*?)\\\\];`));
  console.log('\\n' + name);
  if (match) console.log([...match[1].matchAll(/'([^']+)'/g)].map(m => m[1]).join('\\n'));
}
NODE
```

### 두뇌 이미지 잔존 여부 확인

```bash
rg -n "1677442135703|brain|nextro-innovation-business|nextro-innovation-.*matrix|nextro-innovation-.*tunnel" index.html
```

기대 결과:

- `1677442135703`은 나오면 안 된다.
- `nextro-innovation-business-meeting.mp4`, `nextro-innovation-business-graphs.mp4`는 나오면 안 된다.

## 이어서 작업할 때 순서

1. `cd "/Users/topjh/Desktop/넥스트로 "`
2. `git status -sb`로 미커밋 변경 확인
3. `index.html`의 네 배열 확인
4. 영상 파일을 바꾸면 `.gitignore` 때문에 `git add -f` 필요 여부 확인
5. 로컬에서 `http://localhost:8899/` 확인
6. GitHub에 커밋/푸시
7. Vercel 자동 배포가 안 되면 CLI로 `vercel --prod`
8. 배포 URL에서 캐시 버스터 쿼리로 확인

## 현재 결론

다른 터미널 세션에서 이어서 작업할 때는 이 파일을 먼저 읽으면 된다.

가장 민감한 부분은 4슬라이드다. 두뇌 이미지 문제는 fallback CSS 때문에 발생했으므로, 4슬라이드 fallback에 이미지 URL을 다시 넣지 말아야 한다.
