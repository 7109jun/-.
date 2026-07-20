Jopsydian(좁시디언) JOCP의 동생이지만
더 많은 기능 때문에 더 어려울것입니다'
호불호가 많이 걸릴거라고 예상.
AI가 쓴 메뉴얼 (by Qwen) 그리고 철학은 문서가 곧 콘텐츠가 된다입니다.
# JOCP (Jopsydian) 공식 매뉴얼

**Just Online Comfortable Presentation**  
초보자를 위한 직관적 마크업 언어

---

## 📋 목차

1. [JOCP란?](#jcp란)
2. [기본 문법](#기본-문법)
3. [템플릿 시스템](#템플릿-시스템)
4. [메타데이터](#메타데이터)
5. [파일 시스템](#파일-시스템)
6. [예시 모음](#예시-모음)

---

## JOCP란?

JOCP(좁시디언)는 초보자도 쉽게 사용할 수 있는 직관적 마크업 언어입니다. 복잡한 기호 대신 자연스러운 문법으로 문서, 퀴즈, 갤러리, 동영상, 게임까지 만들 수 있습니다.

### 핵심 철학

- **직관성**: 기호의 개수와 형태로 의미를 직관적으로 표현
- **단순성**: 하나의 기호는 하나의 역할만 수행
- **확장성**: 템플릿 시스템으로 무한한 기능 확장 가능

---

## 기본 문법

### 제목 (Headings)

느낌표(`!`)의 개수로 제목 크기를 조절합니다. **기호가 많을수록 큰 제목**입니다.

```jocp
!!!!!! 대제목 (H1)
!!!!! 중제목 (H2)
!!!! 소제목 (H3)
!!! 4단계 제목
!! 5단계 제목
! 6단계 제목 (가장 작음)
텍스트 서식
~기울임~                    -> 기울임꼴
{굵게}                      -> 은 글씨
_취소선_                    -> 취소선
`밑줄+굵게`                 -> 밑줄과 굵게 동시 적용
?빨간색?<255,0,0>          -> RGB 색상 지정
^반투명^50%                -> 투명도 조절
링크 및 이동
[[https://naver.com]]              -> 일반 링크
=네이버로=[https://naver.com]      -> 하이퍼링크 (텍스트 커스텀)
[대제목][맨 위로 이동]             -> 문서 내 특정 위치로 이동
리스트
순서 없는 리스트 (별표 개수로 중첩)
* 1단계 항목
** 2단계 중첩 항목
*** 3단계 더 깊은 항목
순서 있는 리스트
1 첫 번째 항목
2 두 번째 항목
3 세 번째 항목
표 (Table)
# 이름 # 가격 # 수량
# 포션 # 50골드 # 10개
# 엘릭서 # 120골드 # 5개
체크리스트
+ 미완료 할 일
@ 완료한 일
기타
-                                  -> 구분선 (Horizontal Rule)
&인용구&                           -> 인용 블록
!{이미지URL}                       -> 이미지 삽입
%%이건 주석이라 화면에 안 보임%%   -> 주석
~                                  -> 복사 블록 시작/끝
코드나 텍스트
~
템플릿 시스템
1. 퀴즈 템플릿 (~quiz)
인터랙티브 퀴즈를 만들 수 있습니다.
~quiz
질문: JOCP에서 굵게 기호는 무엇일까요?
- {텍스트} (정답)
- *텍스트*
- **텍스트**
~
특징:
(정답) 표시가 있는 선택지가 정답
클릭 시 정답/오답 시각적 피드백 제공
2. 갤러리 템플릿 (~gallery)
이미지 갤러리를 생성합니다.
~gallery
제목: 내 사진첩
- https://example.com/photo1.jpg
- https://example.com/photo2.jpg
- https://example.com/photo3.jpg
~
특징:
그리드 레이아웃으로 자동 배치
클릭 시 새 탭에서 원본 이미지 열기
3. 미디어 템플릿 (~video, ~audio)
동영상
~video
소스: https://youtu.be/QQyYCAvkOXY
제목: 뽀로로 - 재미있는 모험
~
음악
~audio
소스: https://example.com/song.mp3
제목: 내 플레이리스트
~
지원 형식:
YouTube: https://youtu.be/비디오ID, https://youtube.com/watch?v=비디오ID
MP4: 직접 파일 URL
MP3: 오디오 파일 URL
4. GUI 템플릿 (~gui)
클릭 가능한 버튼들을 생성합니다.
~gui
제목: 제어 패널
- 시작하기: alert('게임 시작!')
- 도움말: alert('JOCP 매뉴얼을 확인하세요.')
- 설정: alert('설정 메뉴')
~
특징:
- 버튼이름: 자바스크립트코드 형식
클릭 시 해당 스크립트 실행
5. 게임 템플릿 (~game)
HTML + CSS + JS로 완전한 게임을 만들 수 있습니다.
~game
제목: 공 튕기기 게임
html:
<div class="game-box">
  <div class="ball"></div>
</div>
css:
.game-box { 
  width: 400px; 
  height: 300px; 
  background: #000; 
  position: relative; 
}
.ball { 
  width: 30px; 
  height: 30px; 
  background: red; 
  border-radius: 50%; 
  position: absolute; 
}
js:
const ball = this.querySelector('.ball');
let x = 0, y = 0, dx = 3, dy = 3;

function animate() {
  x += dx; y += dy;
  if (x > 370 || x < 0) dx = -dx;
  if (y > 270 || y < 0) dy = -dy;
  ball.style.left = x + 'px';
  ball.style.top = y + 'px';
  requestAnimationFrame(animate);
}
animate();
~

구조:
html: - 게임의 HTML 구조 (뼈대)
css: - 게임의 스타일 (디자인)
js: - 게임의 로직 (움직임)
this는 게임 컨테이너를 가리킴
메타데이터
문서 상단에 ~meta 블록으로 문서 정보를 포함합니다.
~meta
제목: JOCP 매뉴얼
저자: 개발자
날짜: 2026-07-18
태그: JOCP, 마크업, 문서, 가이드
설명: JOCP의 모든 기능을 담은 공식 매뉴얼입니다.
버전: 1.0
~
지원 필드:
제목 / title
저자 / author
날짜 / date
태그 / tags
설명 / description
커스텀 필드 자유롭게 추가 가능
특징:
문서 상단에 예쁜 박스로 표시
접기/펼치기 기능 제공
.jpc 내보내기 시 HTML <meta> 태그로 자동 변환 (SEO 최적화)
파일 시스템
파일 확장자
          확장자        이름 설명
jop       JOCP Source   JOCP 문법으로 작성된 소스 코드
jpc     JOCP Compiled   파싱 완료된 독립형 HTML 문서 
저장 및 내보내기
.jop 저장
JOCP 에디터에서 "저장" 버튼 클릭
원본 JOCP 문법이 그대로 저장됨
나중에 다시 불러와서 수정 가능
.jpc 내보내기
"내보내기" 버튼 클릭
JOCP 엔진이 내장된 완전한 HTML 파일 생성
다른 사람에게 공유하면 더블클릭으로 실행 가능
인터넷 연결 없이도 작동 (오프라인 지원)
불러오기
"불러오기" 버튼 클릭
이전에 저장한 .jop 파일 선택
에디터에 내용 자동 로드
예시 모음
예시 1: 블로그 포스트
~meta
제목: JOCP 소개
저자: 홍길동
날짜: 2026-07-18
태그: JOCP, 소개
설명: JOCP가 무엇인지 알아봅니다
~

!!!!!! JOCP란 무엇인가?

JOCP는 ~초보자용~ 마크업 언어입니다.

!!!!! 주요 기능

* 간편한 문법
* 강력한 템플릿
* 게임 엔진

~video
소스: https://youtu.be/QQyYCAvkOXY
제목: JOCP 소개 영상
~
예시 2: 학습 퀴즈
!!!!!! JOCP 학습 테스트

~quiz
질문: JOCP에서 기울임 기호는?
- ~텍스트~ (정답)
- *텍스트*
- _텍스트_
~

~quiz
질문: 제목을 만들 때 사용하는 기호는?
- ! (정답)
- #
- *
~

~gui
제목: 학습 도구
- 정답 확인: alert('모두 정답입니다!')
- 다시하기: alert('처음부터 시작합니다.')
~
예시 3: 포트폴리오
~meta
제목: 내 포트폴리오
저자: 김개발
태그: 포트폴리오, 프로젝트
~

!!!!!! 김개발의 포트폴리오

!!!!! 프로젝트 갤러리

~gallery
제목: 완성된 프로젝트들
- https://example.com/proj1.png
- https://example.com/proj2.png
- https://example.com/proj3.png
~

!!!!! 연락처

이메일: {developer@example.com}

~gui
제목: SNS
- GitHub: window.open('https://github.com')
- LinkedIn: window.open('https://linkedin.com')
~
예시 4: 미니 게임
!!!!!! 클릭 게임

~game
제목: 버튼 클릭 게임
html:
<div class="game-area">
  <button id="clickBtn">클릭!</button>
  <p>점수: <span id="score">0</span></p>
</div>
css:
.game-area { 
  text-align: center; 
  padding: 50px; 
  background: #f0f0f0; 
}
#clickBtn { 
  font-size: 24px; 
  padding: 20px 40px; 
  cursor: pointer;
  background: #4a9eff;
  color: white;
  border: none;
  border-radius: 8px;
}
#score {
  font-size: 32px;
  font-weight: bold;
  color: #4a9eff;
}
js:
let score = 0;
const btn = this.querySelector('#clickBtn');
const scoreEl = this.querySelector('#score');

btn.addEventListener('click', () => {
  score++;
  scoreEl.textContent = score;
});
~
추가 리소스
GitHub 저장소: JOCP 공식 저장소(https://github.com/7109jun/-./new/main)
이슈 제보: 버그나 기능 요청은 Issues 탭에서
📄 라이선스
JOCP는 MIT 라이선스 하에 배포됩니다.
만든 이: JOCP 개발팀은 걔뿔 나 혼자 만듬
버전: 5.2
마지막 업데이트: 2026-07-18
코드:
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>JOCP V5.2 - Jopsydian Engine</title>
  <style>
    body { font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, sans-serif; max-width: 1400px; margin: 0 auto; padding: 20px; background: #f8f9fa; color: #333; }
    .toolbar { display: flex; gap: 10px; margin-bottom: 15px; padding-bottom: 15px; border-bottom: 2px solid #e9ecef; }
    .btn { padding: 8px 16px; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; font-size: 14px; transition: 0.2s; color: white; }
    .btn-save { background: #4a9eff; }
    .btn-export { background: #2ecc71; }
    .btn-load { background: #9b59b6; }
    .btn:hover { opacity: 0.85; transform: translateY(-1px); }
    
    .editor-container { display: flex; gap: 20px; height: 80vh; }
    textarea { flex: 1; height: 100%; font-family: 'Fira Code', monospace; font-size: 14px; padding: 15px; border: 1px solid #ced4da; border-radius: 8px; resize: none; outline: none; }
    textarea:focus { border-color: #4a9eff; }
    #preview { flex: 1; height: 100%; overflow-y: auto; padding: 25px; border: 1px solid #ced4da; border-radius: 8px; background: #ffffff; box-shadow: 0 2px 8px rgba(0,0,0,0.05); }

    /* 메타데이터 스타일 */
    .jocp-meta-box { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 20px; border-radius: 12px; margin-bottom: 25px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
    .jocp-meta-title { font-size: 2em; font-weight: bold; margin-bottom: 10px; }
    .jocp-meta-info { display: flex; gap: 20px; flex-wrap: wrap; font-size: 0.9em; opacity: 0.9; margin-bottom: 15px; }
    .jocp-meta-item { display: flex; align-items: center; gap: 5px; }
    .jocp-meta-desc { font-size: 1.05em; line-height: 1.6; opacity: 0.95; }
    .jocp-meta-toggle { margin-top: 15px; padding: 8px 16px; background: rgba(255,255,255,0.2); border: none; border-radius: 6px; color: white; cursor: pointer; font-size: 0.9em; }
    .jocp-meta-toggle:hover { background: rgba(255,255,255,0.3); }
    .jocp-meta-details { margin-top: 15px; padding-top: 15px; border-top: 1px solid rgba(255,255,255,0.3); font-size: 0.9em; }
    .jocp-meta-details.hidden { display: none; }

    h1, h2, h3, h4, h5, h6 { margin-top: 24px; margin-bottom: 12px; color: #212529; }
    p { line-height: 1.6; margin-bottom: 12px; }
    
    .jocp-copy-block { position: relative; background: #212529; color: #f8f9fa; padding: 15px; border-radius: 8px; margin: 15px 0; }
    .jocp-copy-block pre { margin: 0; white-space: pre-wrap; font-family: monospace; }
    .copy-btn { position: absolute; top: 10px; right: 10px; background: #4a9eff; color: white; border: none; padding: 6px 12px; border-radius: 4px; cursor: pointer; font-size: 12px; }
    
    .jocp-quiz { background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 8px; padding: 20px; margin: 15px 0; }
    .jocp-quiz-title { font-weight: bold; margin-bottom: 15px; font-size: 1.1em; color: #495057; }
    .jocp-quiz-option { display: block; padding: 10px 15px; margin: 8px 0; border: 1px solid #dee2e6; border-radius: 6px; cursor: pointer; background: white; transition: 0.2s; }
    .jocp-quiz-option:hover { background: #e9ecef; }
    .jocp-quiz-option.correct { background: #d4edda; border-color: #c3e6cb; color: #155724; }
    .jocp-quiz-option.wrong { background: #f8d7da; border-color: #f5c6cb; color: #721c24; }
    
    .jocp-gallery { background: #fff; border: 1px solid #dee2e6; border-radius: 8px; padding: 20px; margin: 15px 0; }
    .jocp-gallery-title { font-weight: bold; margin-bottom: 15px; font-size: 1.1em; }
    .jocp-gallery-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 15px; }
    .jocp-gallery-grid img { width: 100%; height: 150px; object-fit: cover; border-radius: 6px; cursor: pointer; transition: 0.2s; }
    .jocp-gallery-grid img:hover { transform: scale(1.03); box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
    
    .jocp-media { background: #fff; border: 1px solid #dee2e6; border-radius: 8px; padding: 20px; margin: 15px 0; }
    .jocp-media-title { font-weight: bold; margin-bottom: 15px; font-size: 1.1em; }
    .jocp-media video, .jocp-media audio { width: 100%; max-width: 100%; border-radius: 6px; }
    .jocp-media iframe { width: 100%; aspect-ratio: 16/9; border-radius: 6px; border: none; }
    
    .jocp-gui { background: #f8f9fa; border: 1px solid #dee2e6; border-radius: 8px; padding: 20px; margin: 15px 0; }
    .jocp-gui-title { font-weight: bold; margin-bottom: 15px; }
    .jocp-gui-btn { display: inline-block; padding: 10px 20px; margin: 5px; background: #4a9eff; color: white; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; transition: 0.2s; }
    .jocp-gui-btn:hover { background: #3a8eef; }
    
    .jocp-game { background: #fff; border: 1px solid #dee2e6; border-radius: 8px; padding: 20px; margin: 15px 0; }
    .jocp-game-title { font-weight: bold; margin-bottom: 15px; }
    .jocp-game-container { border: 2px solid #333; border-radius: 6px; overflow: hidden; background: #000; }

    .jocp-checkbox { display: block; margin: 8px 0; cursor: pointer; }
    .jocp-checkbox.completed { color: #adb5bd; text-decoration: line-through; }
    hr { border: none; border-top: 2px solid #e9ecef; margin: 24px 0; }
    table { border-collapse: collapse; width: 100%; margin: 15px 0; }
    th, td { border: 1px solid #dee2e6; padding: 10px 15px; text-align: left; }
    th { background: #f8f9fa; font-weight: bold; }
    ul ul, ol ol { margin-left: 20px; }
    a { color: #4a9eff; text-decoration: none; }
    a:hover { text-decoration: underline; }
    blockquote { border-left: 4px solid #4a9eff; padding: 10px 20px; background: #f8f9fa; margin: 15px 0; border-radius: 0 6px 6px 0; }
  </style>
</head>
<body>

  <div class="toolbar">
    <button class="btn btn-save" onclick="downloadJOP()">💾 .jop 저장</button>
    <button class="btn btn-export" onclick="downloadJPC()">🚀 .jpc 내보내기</button>
    <button class="btn btn-load" onclick="document.getElementById('file-input').click()"> .jop 불러오기</button>
    <input type="file" id="file-input" accept=".jop" style="display:none" onchange="loadJOP(event)">
  </div>

  <div class="editor-container">
    <textarea id="jocp-input" oninput="render()">~meta
제목: JOCP V5.2 - 뽀로로와 함께하는 문서 엔진
저자: 개발자
날짜: 2026-07-18
태그: JOCP, 마크업, 문서, 게임, 뽀로로
설명: JOCP는 초보자를 위한 직관적 마크업 언어입니다. 문서, 즈, 갤러리, 유튜브, 게임까지 모두 만들 수 있습니다!
~

!!!!!! JOCP V5.2 - 완전한 문서 및 앱 엔진
!!!!! 1. 기본 문법 테스트
이것은 ~기울임~이고, {굵게}이며, `밑줄+게` 입니다.
취소선은 _이렇게_ 그어집니다.
색상은 ?빨간색?<255,0,0> ?파란색?<0,0,255> 이고, 투명도는 ^반투명^50% 입니다.

!!!!! 2. 미디어 (유튜브 & MP4 통합)
~video
소스: https://youtu.be/QQyYCAvkOXY?si=55DPs74YDEPUgjLw
제목: 로로 - 재미있는 모험
~

~audio
소스: https://www.w3schools.com/html/horse.mp3
제목: 샘플 오디오
~

!!!!! 3. 인터랙티브 템플릿
~quiz
질문: JOCP에서 굵게 기호는 무엇일까요?
- {텍스트} (정답)
- *텍스트*
- **텍스트**
~

~gallery
제목: 샘플 갤러리
- https://via.placeholder.com/400x300/FF6B6B/FFFFFF?text=Photo+1
- https://via.placeholder.com/400x300/4ECDC4/FFFFFF?text=Photo+2
- https://via.placeholder.com/400x300/45B7D1/FFFFFF?text=Photo+3
~

~gui
제목: 제어 패널
- 시작: alert('시스템이 시작되었습니다!')
- 도움말: alert('JOCP 문법을 사용해 보세요.')
~

!!!!! 4. 게임 엔진 (HTML + CSS + JS)
~game
제목: 튕기는 공 게임
html:
<div class="game-box">
  <div class="ball"></div>
</div>
css:
.game-box { width: 100%; height: 300px; background: #212529; position: relative; }
.ball { width: 30px; height: 30px; background: #ff6b6b; border-radius: 50%; position: absolute; top: 0; left: 0; }
js:
const ball = this.querySelector('.ball');
let x = 0, y = 0, dx = 4, dy = 4;
function animate() {
  x += dx; y += dy;
  const box = this.parentElement;
  if (x > box.offsetWidth - 30 || x < 0) dx = -dx;
  if (y > box.offsetHeight - 30 || y < 0) dy = -dy;
  ball.style.left = x + 'px';
  ball.style.top = y + 'px';
  requestAnimationFrame(animate);
}
animate();
~

!!!!! 5. 기타 구조
# 이름 # 가격 # 재고
# 포션 # 50골드 # 10개
# 엘릭서 # 120골드 # 5개

* 1단계 항목
** 2단계 중첩 항목
*** 3단계 더 깊은 항목

+ 아직 안 한 일
@ 이미 완료한 일

~
console.log("이건 복사 블록입니다!");
~
</textarea>
    <div id="preview"></div>
  </div>

  <script>
    const JOCP = (function() {
      'use strict';

      const REGEX = {
        heading: /^(!+)\s+(.*)$/,
        table: /^#\s/,
        listUnordered: /^(\*+)\s+(.*)$/,
        listOrdered: /^(\d+)\s+(.*)$/,
        checkboxTodo: /^\+\s+(.*)$/,
        checkboxDone: /^@\s+(.*)$/,
        hr: /^-\s*$/,
        copyBlock: /^~\s*$/,
        metaBlock: /^~meta\s*$/,
        
        templateQuiz: /^~quiz\s*$/,
        templateGallery: /^~gallery\s*$/,
        templateVideo: /^~video\s*$/,
        templateAudio: /^~audio\s*$/,
        templateGUI: /^~gui\s*$/,
        templateGame: /^~game\s*$/,

        inlineComment: /%%[^%]*%%/g,
        inlineImage: /!\{([^}]+)\}/g,
        inlineHyperlink: /=([^=]+)=\[([^\]]+)\]/g,
        inlineAnchor: /\[([^\]]+)\]\[([^\]]+)\]/g,
        inlineLink: /\[\[([^\]]+)\]\]/g,
        inlineQuote: /&([^&]+)&/g,
        inlineBoldUnder: /`([^`]+)`/g,
        inlineColor: /\?([^?]+)\?<(\d+,\d+,\d+)>/g,
        inlineOpacity: /\^([^^]+)\^(\d+)%/g,
        inlineBold: /\{([^}]+)\}/g,
        inlineItalic: /~([^~]+)~/g,
        inlineDel: /_([^_]+)_/g
      };

      let metaData = {};

      function escapeHtml(text) {
        const map = { '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;', "'": '&#039;' };
        return text.replace(/[&<>"']/g, m => map[m]);
      }

      function slugify(text) {
        return text.replace(/\s+/g, '-').replace(/[^\w-가-]/g, '');
      }

      function parseInline(text) {
        const tokens = [];
        const store = (html) => {
          const token = `§JOCP${tokens.length}§`;
          tokens.push(html);
          return token;
        };

        text = text.replace(REGEX.inlineComment, '');
        text = text.replace(REGEX.inlineImage, (m, src) => store(`<img src="${src}" alt="image" style="max-width:100%; border-radius:4px;" />`));
        text = text.replace(REGEX.inlineHyperlink, (m, label, url) => store(`<a href="${url}" target="_blank">${label}</a>`));
        text = text.replace(REGEX.inlineAnchor, (m, target, label) => store(`<a href="#${slugify(target)}" style="background:#e7f5ff; padding:2px 6px; border-radius:4px; border:1px solid #4a9eff;">${label}</a>`));
        text = text.replace(REGEX.inlineLink, (m, url) => store(`<a href="${url}" target="_blank">${url}</a>`));
        text = text.replace(REGEX.inlineQuote, (m, content) => store(`<blockquote>${content}</blockquote>`));
        text = text.replace(REGEX.inlineBoldUnder, (m, content) => store(`<ins><strong>${content}</strong></ins>`));
        text = text.replace(REGEX.inlineColor, (m, content, rgb) => store(`<span style="color:rgb(${rgb})">${content}</span>`));
        text = text.replace(REGEX.inlineOpacity, (m, content, percent) => store(`<span style="opacity:${percent/100}">${content}</span>`));
        text = text.replace(REGEX.inlineBold, (m, content) => store(`<strong>${content}</strong>`));
        text = text.replace(REGEX.inlineItalic, (m, content) => store(`<em>${content}</em>`));
        text = text.replace(REGEX.inlineDel, (m, content) => store(`<del>${content}</del>`));

        return text.replace(/§JOCP(\d+)§/g, (m, idx) => tokens[parseInt(idx)]);
      }

      // 메타데이터 파서
      function parseMetaTemplate(lines, startIdx) {
        metaData = {};
        let i = startIdx + 1;
        while (i < lines.length && lines[i].trim() !== '~') {
          const line = lines[i].trim();
          const colonIdx = line.indexOf(':');
          if (colonIdx !== -1) {
            const key = line.substring(0, colonIdx).trim();
            const value = line.substring(colonIdx + 1).trim();
            metaData[key] = value;
          }
          i++;
        }
        return { nextIdx: i + 1 };
      }

      function renderMetaData() {
        if (Object.keys(metaData).length === 0) return '';
        
        const title = metaData.제목 || metaData.title || '제목 없음';
        const author = metaData.저자 || metaData.author || '';
        const date = metaData.날짜 || metaData.date || '';
        const tags = metaData.태그 || metaData.tags || '';
        const desc = metaData.설명 || metaData.description || '';

        let html = `<div class="jocp-meta-box">
                      <div class="jocp-meta-title">${escapeHtml(title)}</div>
                      <div class="jocp-meta-info">`;
        
        if (author) html += `<div class="jocp-meta-item">✍️ ${escapeHtml(author)}</div>`;
        if (date) html += `<div class="jocp-meta-item">📅 ${escapeHtml(date)}</div>`;
        if (tags) html += `<div class="jocp-meta-item">🏷️ ${escapeHtml(tags)}</div>`;
        
        html += `</div>`;
        
        if (desc) {
          html += `<div class="jocp-meta-desc">${escapeHtml(desc)}</div>`;
        }
        
        html += `<button class="jocp-meta-toggle" onclick="this.nextElementSibling.classList.toggle('hidden'); this.textContent = this.nextElementSibling.classList.contains('hidden') ? '📋 메타데이터 보기' : '📋 메타데이터 숨기기';">📋 메타데이터 숨기기</button>
                 <div class="jocp-meta-details">`;
        
        for (const [key, value] of Object.entries(metaData)) {
          if (!['제목', 'title', '저자', 'author', '날짜', 'date', '태그', 'tags', '설명', 'description'].includes(key)) {
            html += `<div><strong>${escapeHtml(key)}:</strong> ${escapeHtml(value)}</div>`;
          }
        }
        
        html += `</div></div>`;
        return html;
      }

      function parseQuizTemplate(lines, startIdx) {
        let i = startIdx + 1;
        let question = '', options = [], correctIndex = -1;
        while (i < lines.length && lines[i].trim() !== '~') {
          const line = lines[i].trim();
          if (line.startsWith('질문:')) question = line.replace('질문:', '').trim();
          else if (line.startsWith('- ')) {
            const optText = line.substring(2);
            if (optText.includes('(정답)')) { correctIndex = options.length; options.push(optText.replace('(정답)', '').trim()); }
            else { options.push(optText); }
          }
          i++;
        }
        let html = `<div class="jocp-quiz"><div class="jocp-quiz-title">📝 ${parseInline(question)}</div>`;
        options.forEach((opt, idx) => {
          html += `<div class="jocp-quiz-option" onclick="JOCP.checkQuiz(this, ${idx}, ${correctIndex})">${parseInline(opt)}</div>`;
        });
        return { html: html + '</div>', nextIdx: i + 1 };
      }

      function parseGalleryTemplate(lines, startIdx) {
        let i = startIdx + 1;
        let title = '갤러리', images = [];
        while (i < lines.length && lines[i].trim() !== '~') {
          const line = lines[i].trim();
          if (line.startsWith('제목:')) title = line.replace('제목:', '').trim();
          else if (line.startsWith('- ')) images.push(line.substring(2).trim());
          i++;
        }
        let html = `<div class="jocp-gallery"><div class="jocp-gallery-title">️ ${parseInline(title)}</div><div class="jocp-gallery-grid">`;
        images.forEach(img => { html += `<img src="${img}" alt="gallery" onclick="window.open('${img}', '_blank')" />`; });
        return { html: html + '</div></div>', nextIdx: i + 1 };
      }

      function parseVideoTemplate(lines, startIdx) {
        let i = startIdx + 1;
        let source = '', title = '동영상';
        while (i < lines.length && lines[i].trim() !== '~') {
          const line = lines[i].trim();
          if (line.startsWith('소스:')) source = line.replace('소스:', '').trim();
          else if (line.startsWith('제목:')) title = line.replace('제목:', '').trim();
          i++;
        }
        
        let mediaHTML = '';
        // 유튜브 URL 추출 (개선된 정규식 - 파라미터 제거)
        const ytMatch = source.match(/(?:youtube\.com\/(?:[^\/]+\/.+\/|(?:v|e(?:mbed)?)\/|.*[?&]v=)|youtu\.be\/)([^"&?\/\s]{11})/);
        
        if (ytMatch) {
          // 비디오 ID만 사용하여 깨끗한 임베드 URL 생성
          const videoId = ytMatch[1];
          mediaHTML = `<iframe src="https://www.youtube.com/embed/${videoId}?rel=0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>`;
        } else {
          mediaHTML = `<video controls src="${source}"></video>`;
        }
        
        return { html: `<div class="jocp-media"><div class="jocp-media-title"> ${parseInline(title)}</div>${mediaHTML}</div>`, nextIdx: i + 1 };
      }

      function parseAudioTemplate(lines, startIdx) {
        let i = startIdx + 1;
        let source = '', title = '음악';
        while (i < lines.length && lines[i].trim() !== '~') {
          const line = lines[i].trim();
          if (line.startsWith('소스:')) source = line.replace('소스:', '').trim();
          else if (line.startsWith('제목:')) title = line.replace('제목:', '').trim();
          i++;
        }
        return { html: `<div class="jocp-media"><div class="jocp-media-title">🎵 ${parseInline(title)}</div><audio controls src="${source}"></audio></div>`, nextIdx: i + 1 };
      }

      function parseGUITemplate(lines, startIdx) {
        let i = startIdx + 1;
        let title = 'GUI', buttons = [];
        while (i < lines.length && lines[i].trim() !== '~') {
          const line = lines[i].trim();
          if (line.startsWith('제목:')) title = line.replace('제목:', '').trim();
          else if (line.startsWith('- ')) {
            const btnText = line.substring(2);
            const colonIdx = btnText.indexOf(':');
            if (colonIdx !== -1) buttons.push({ label: btnText.substring(0, colonIdx).trim(), code: btnText.substring(colonIdx + 1).trim() });
          }
          i++;
        }
        let html = `<div class="jocp-gui"><div class="jocp-gui-title">🎛️ ${parseInline(title)}</div>`;
        buttons.forEach((btn, idx) => {
          html += `<button class="jocp-gui-btn" onclick="JOCP.executeScript(${idx})">${parseInline(btn.label)}</button>`;
          JOCP.scriptStore.push(btn.code);
        });
        return { html: html + '</div>', nextIdx: i + 1 };
      }

      function parseGameTemplate(lines, startIdx) {
        let i = startIdx + 1;
        let title = '게임', htmlContent = '', cssContent = '', jsContent = '', currentSection = '';
        while (i < lines.length && lines[i].trim() !== '~') {
          const line = lines[i];
          if (line.startsWith('제목:')) title = line.replace('제목:', '').trim();
          else if (line.trim() === 'html:') currentSection = 'html';
          else if (line.trim() === 'css:') currentSection = 'css';
          else if (line.trim() === 'js:') currentSection = 'js';
          else if (currentSection === 'html') htmlContent += line + '\n';
          else if (currentSection === 'css') cssContent += line + '\n';
          else if (currentSection === 'js') jsContent += line + '\n';
          i++;
        }
        const gameId = 'game_' + Math.random().toString(36).substr(2, 9);
        const scriptId = JOCP.scriptStore.length;
        JOCP.scriptStore.push(jsContent);

        const html = `<div class="jocp-game" id="${gameId}">
                        <div class="jocp-game-title">🎮 ${parseInline(title)}</div>
                        <style>${cssContent}</style>
                        <div class="jocp-game-container">${htmlContent}</div>
                        <script>
                          setTimeout(() => {
                            const el = document.getElementById('${gameId}').querySelector('.jocp-game-container');
                            if (el) { try { new Function(JOCP.scriptStore[${scriptId}]).call(el); } catch(e) { console.error('Game error:', e); } }
                          }, 100);
                        <\/script>
                      </div>`;
        return { html, nextIdx: i + 1 };
      }

      function parseBlock(lines) {
        let html = '', i = 0;
        let metaRendered = false;
        
        while (i < lines.length) {
          const line = lines[i];
          
          // 메타데이터 블록 (문서 최상단에서만 처리)
          if (REGEX.metaBlock.test(line) && !metaRendered) {
            const r = parseMetaTemplate(lines, i);
            html += renderMetaData();
            i = r.nextIdx;
            metaRendered = true;
            continue;
          }
          
          if (REGEX.templateQuiz.test(line)) { const r = parseQuizTemplate(lines, i); html += r.html; i = r.nextIdx; continue; }
          if (REGEX.templateGallery.test(line)) { const r = parseGalleryTemplate(lines, i); html += r.html; i = r.nextIdx; continue; }
          if (REGEX.templateVideo.test(line)) { const r = parseVideoTemplate(lines, i); html += r.html; i = r.nextIdx; continue; }
          if (REGEX.templateAudio.test(line)) { const r = parseAudioTemplate(lines, i); html += r.html; i = r.nextIdx; continue; }
          if (REGEX.templateGUI.test(line)) { const r = parseGUITemplate(lines, i); html += r.html; i = r.nextIdx; continue; }
          if (REGEX.templateGame.test(line)) { const r = parseGameTemplate(lines, i); html += r.html; i = r.nextIdx; continue; }
          
          if (REGEX.copyBlock.test(line)) {
            let content = ''; i++;
            while (i < lines.length && !REGEX.copyBlock.test(lines[i])) { content += lines[i] + '\n'; i++; }
            i++;
            html += `<div class="jocp-copy-block"><button class="copy-btn" onclick="JOCP.copyText(this)">복사</button><pre><code>${escapeHtml(content.trim())}</code></pre></div>`;
            continue;
          }
          if (REGEX.table.test(line)) {
            let rows = [];
            while (i < lines.length && REGEX.table.test(lines[i])) { rows.push(lines[i].split('#').map(c => c.trim()).filter(c => c !== '')); i++; }
            html += '<table>' + rows.map((row, idx) => `<tr>${row.map(cell => `<${idx===0?'th':'td'}>${parseInline(cell)}</${idx===0?'th':'td'}>`).join('')}</tr>`).join('') + '</table>';
            continue;
          }
          if (REGEX.listUnordered.test(line)) {
            let listHtml = '<ul>', prevLevel = 0;
            while (i < lines.length && REGEX.listUnordered.test(lines[i])) {
              const match = lines[i].match(REGEX.listUnordered);
              const level = match[1].length;
              if (level > prevLevel) for (let j = 0; j < level - prevLevel; j++) listHtml += '<ul>';
              else if (level < prevLevel) for (let j = 0; j < prevLevel - level; j++) listHtml += '</ul>';
              listHtml += `<li>${parseInline(match[2])}</li>`;
              prevLevel = level; i++;
            }
            for (let j = 0; j < prevLevel; j++) listHtml += '</ul>';
            html += listHtml; continue;
          }
          if (REGEX.listOrdered.test(line)) {
            html += '<ol>';
            while (i < lines.length && REGEX.listOrdered.test(lines[i])) {
              html += `<li>${parseInline(lines[i].replace(REGEX.listOrdered, '$2'))}</li>`; i++;
            }
            html += '</ol>'; continue;
          }
          if (REGEX.heading.test(line)) {
            const match = line.match(REGEX.heading);
            html += `<h${7 - match[1].length} id="${slugify(match[2])}">${parseInline(match[2])}</h${7 - match[1].length}>`; i++; continue;
          }
          if (REGEX.hr.test(line)) { html += '<hr />'; i++; continue; }
          if (REGEX.checkboxTodo.test(line)) { html += `<label class="jocp-checkbox"><input type="checkbox"> ${parseInline(line.replace(REGEX.checkboxTodo, '$1'))}</label>`; i++; continue; }
          if (REGEX.checkboxDone.test(line)) { html += `<label class="jocp-checkbox completed"><input type="checkbox" checked disabled> ${parseInline(line.replace(REGEX.checkboxDone, '$1'))}</label>`; i++; continue; }
          
          if (line.trim() === '') { i++; continue; }
          html += `<p>${parseInline(line)}</p>`; i++;
        }
        return html;
      }

      return {
        scriptStore: [],
        metaData: metaData,
        parse: function(text) { this.scriptStore = []; metaData = {}; return parseBlock(text.split('\n')); },
        checkQuiz: function(el, sel, cor) {
          const opts = el.parentElement.querySelectorAll('.jocp-quiz-option');
          opts.forEach((opt, idx) => {
            opt.classList.remove('correct', 'wrong');
            if (idx === cor) opt.classList.add('correct');
            else if (idx === sel) opt.classList.add('wrong');
          });
        },
        executeScript: function(id) {
          const code = this.scriptStore[id];
          if (code) { try { new Function(code)(); } catch (e) { alert('스크립트 오류: ' + e.message); } }
        },
        copyText: function(btn) {
          navigator.clipboard.writeText(btn.parentElement.querySelector('code').innerText).then(() => {
            btn.textContent = '복사 완료! ✔'; setTimeout(() => { btn.textContent = '복사'; }, 1500);
          });
        },
        render: function(inputId, previewId) {
          const input = document.getElementById(inputId), preview = document.getElementById(previewId);
          if (!input || !preview) return;
          const update = () => { preview.innerHTML = this.parse(input.value); };
          input.addEventListener('input', update); update();
          preview.addEventListener('click', (e) => {
            if (e.target.classList.contains('jocp-anchor') || e.target.parentElement?.classList.contains('jocp-anchor')) {
              e.preventDefault();
              const target = preview.querySelector(e.target.getAttribute('href') || e.target.parentElement.getAttribute('href'));
              if (target) target.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }
          });
        }
      };
    })();

    function downloadJOP() {
      const blob = new Blob([document.getElementById('jocp-input').value], { type: 'text/plain' });
      const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = 'document.jop'; a.click(); URL.revokeObjectURL(a.href);
    }

    function downloadJPC() {
      const source = document.getElementById('jocp-input').value;
      const rendered = JOCP.parse(source);
      const engine = JOCP.toString();
      const meta = JOCP.metaData;
      const metaTags = Object.entries(meta).map(([k,v]) => `<meta name="${k}" content="${v}">`).join('\n');
      
      const html = `<!DOCTYPE html><html><head><meta charset="UTF-8"><title>${meta.제목 || meta.title || 'JOCP Document'}</title>
      ${metaTags}
      <style>body{font-family:sans-serif;max-width:800px;margin:0 auto;padding:20px;}
      .jocp-meta-box{background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);color:white;padding:20px;border-radius:12px;margin-bottom:25px;}
      .jocp-meta-title{font-size:2em;font-weight:bold;margin-bottom:10px;}
      .jocp-quiz,.jocp-gallery,.jocp-media,.jocp-gui,.jocp-game{background:#f8f9fa;border:1px solid #dee2e6;border-radius:8px;padding:20px;margin:15px 0;}
      .jocp-quiz-option{display:block;padding:10px;margin:8px 0;border:1px solid #dee2e6;border-radius:6px;cursor:pointer;background:white;}
      .jocp-quiz-option.correct{background:#d4edda;border-color:#c3e6cb;} .jocp-quiz-option.wrong{background:#f8d7da;border-color:#f5c6cb;}
      .jocp-gallery-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:15px;} .jocp-gallery-grid img{width:100%;height:150px;object-fit:cover;border-radius:6px;}
      .jocp-media video,.jocp-media audio,.jocp-media iframe{width:100%;border-radius:6px;} .jocp-gui-btn{display:inline-block;padding:10px 20px;margin:5px;background:#4a9eff;color:white;border:none;border-radius:6px;cursor:pointer;}
      .jocp-game-container{border:2px solid #333;border-radius:6px;overflow:hidden;background:#000;}</style></head>
      <body><div id="app">${rendered}</div><script>const JOCP=(${engine})();<\/script></body></html>`;
      const a = document.createElement('a'); a.href = URL.createObjectURL(new Blob([html], { type: 'text/html' })); a.download = 'document.jpc'; a.click(); URL.revokeObjectURL(a.href);
    }

    function loadJOP(event) {
      const file = event.target.files[0]; if (!file) return;
      const reader = new FileReader();
      reader.onload = (e) => { document.getElementById('jocp-input').value = e.target.result; document.getElementById('jocp-input').dispatchEvent(new Event('input')); };
      reader.readAsText(file);
    }

    function render() { JOCP.render('jocp-input', 'preview'); }
    document.addEventListener('DOMContentLoaded', render);
  </script>
</body>
</html>

