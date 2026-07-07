# NEXTRO 프로젝트 — Claude Code 운영 지침

## 프로젝트 개요
- **회사**: NEXTRO (AI 솔루션 스타트업)
- **현재 작업**: 기업 홈페이지 (`/Users/topjh/Desktop/넥스트로 /index.html`)
- **디자인 레퍼런스**: POSCO DX (https://www.poscodx.com/kor) 스타일 기반

## 기술 스택
- 단일 HTML 파일 (index.html) — 외부 빌드 도구 없음
- Vanilla CSS + JS (프레임워크 없음)
- Google Fonts: Noto Sans KR + Inter

## 디자인 시스템
```css
--navy: #0B1F3A      /* 메인 다크 */
--blue: #1B5EA1      /* 포인트 블루 */
--light-blue: #EEF4F9 /* 섹션 배경 */
--teal: #2DCFAE      /* 브랜드 포인트 */
--body-text: #4A6070
```

## 코딩 규칙
- 주석은 WHY가 명확할 때만 — 무의미한 주석 금지
- 인라인 스타일 사용 금지 — 반드시 CSS 클래스로
- JS는 파일 하단 `<script>` 블록에 모두 작성
- 이미지: Unsplash CDN (`?w=600&q=80&auto=format&fit=crop`)

## 애니메이션 원칙
- 스크롤 리빌: `opacity:0 + translateY(40px)` → `in` 클래스로 전환
- IntersectionObserver threshold: 0.1
- transition: 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94)
- stagger delay: 요소별 100ms 간격

## 금지 사항
- 불필요한 파일 생성 금지 (*.md 문서, README 등)
- 빌드 시스템/번들러 도입 금지
- 외부 JS 라이브러리 추가 금지 (현재 없음 유지)
- `align-items: stretch` 로 인한 버튼 풀-width 문제 주의

## Git 커밋 규칙 (필수)
- **절대 금지**: `Co-Authored-By: Claude` 줄을 커밋 메시지에 추가하지 말 것
- 모든 커밋의 author는 반드시 `전현승 <ovzexx@gmail.com>` 단독으로
- `git commit` 시 `-m` 플래그 사용 시 Co-Authored-By 없이 메시지만 작성
- git config: `user.email = ovzexx@gmail.com`, `user.name = 전현승`

## NEXTRO 콘텐츠 레퍼런스
- 홈페이지 초안 이미지: `/Users/topjh/Desktop/넥스트로 /홈페이지 초안/`
- 실제 사이트 참고: nextrocorp.kr
- 로고: 인라인 SVG (teal 번개 폴리곤 + NEXTRO 워드마크)

## NEXTRO 주요 제품
- **AIM**: 예측 정비 AI 솔루션
- **EcoVision AI**: 환경 모니터링
- **어플무브**: 모바일 솔루션
- **맞춤형 AI**: 산업별 커스텀

## 주요 고객사
한국중부발전, 두산에너빌리티, 대전광역시, ETRI, KETI, 보건복지부

## 작업 시작 전 체크리스트
1. index.html 현재 상태 확인 (Read)
2. 변경 범위 명확히 한 후 편집
3. 브라우저에서 실제 확인 후 완료 보고

## 서버 실행
```bash
cd "/Users/topjh/Desktop/넥스트로 "
python3 -m http.server 8899
```
브라우저: http://localhost:8899/
