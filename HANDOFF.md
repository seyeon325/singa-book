# 신가1리 이야기 — 웹사이트 인수인계 노트

## 파일 구성
- `index.html` — 완성된 단일 HTML 파일 (원본 `singa-book.html`을 리네임). 인라인 CSS/JS만 사용하고 외부 의존성은 Google Fonts(Jua, Gowun Dodum, Gaegu) 하나뿐입니다.
- 빌드 과정 없음 (React·번들러·npm 없음). 더블클릭해서 브라우저로 바로 열리고, `file://` 경로에서도 정상 동작합니다.
- git 저장소로 관리됩니다 (`~/source/repos/singa-book`, GitHub: `seyeon325/singa-book`).

## 배포
- **공개 주소: https://seyeon325.github.io/singa-book/**
- GitHub Pages, `main` 브랜치 루트에서 자동 배포. `main`에 push하면 1~2분 뒤 반영됩니다.
- 수정 → 배포: `git add -A && git commit -m "..." && git push`

## 구조 (하이브리드: 표지 → 스크롤 저널)
- **표지(`.cover`)** — 책상 위에 놓인 다이어리. `펼쳐보기` 버튼 → `.journal` 로 전환하며 스크롤 시작
- **저널(`.journal`)** — 세로 스크롤. 챕터 하나 = `<section class="chapter" id="c-...">`
  - 순서: `c-prologue` → `c-village` → `c-glance` → `c-day1` → `c-day2` → `c-teams` → `c-stories` → `c-credits`
  - 각 챕터: 킥커(`.ch-kicker`) + 제목(`.ch-title`) + 큰 사진(`figure.print`) + 본문
- 상단 sticky 바: 홈 버튼(표지로) + 챕터 이동 드롭다운(`#jump`)
- URL 해시(`#c-village`)로 특정 챕터 링크 공유 / 새로고침 복원 / 뒤로가기. 스크롤하면 해시·드롭다운·진행바 자동 갱신
- `Esc` 또는 "처음으로 돌아가기" → 표지로

## 디자인 토큰 (`:root` CSS 변수)
| 변수 | 값 | 용도 |
|---|---|---|
| `--leaf` / `--leaf-deep` | #3AA95E / #1C7A3E | 메인 그린 |
| `--bg` | #E7DFC6 | 바깥 배경 |
| `--paper` / `--paper-warm` | #FBF6E8 / #F5EDD8 | 종이색 |
| `--ink` / `--ink-soft` | #26331D / #5E6B52 | 텍스트 |
| `--butter` `--sky` `--blush` `--sage` | 파스텔 4색 | 태그·카드 로테이션 |
| `--paper-tex` | SVG feDiffuseLighting | 절차적 "실사풍" 종이 엠보싱 (외부 이미지 0개) |

폰트: 제목 `Jua`, 본문 `Gowun Dodum`, 손글씨 `Gaegu` / `Nanum Pen Script`.

## 톤
리소 프린트 / 플랫 컷페이퍼 셰이프 (전주도서관 여행 포스터 레퍼런스). 크림 종이 + 리소 그레인 오버레이(`.grain`), 제한 팔레트(green/blue/yellow/coral/black), 손글씨(Gaegu·Nanum Pen Script). 셰이프는 `<svg><use href="#sh-...">` (treecloud/tree/book/pencil/aster/flower/drop/blob/arrow/face/camera).
- 표지: 크림 배경에 플랫 셰이프 산개 배치(`.cv-s1`~`.cv-s7` 위치 조정), 큰 손글씨 제목
- 사진(`figure.shot`): 뒤에 컬러 블록(`--mount`) 깔린 플랫 인화 (폴라로이드·그림자 없음)

## 마을 이야기 — 3 x 5 = 15명
스크립트 `STORIES` 배열 15개. 각 항목 `{ name, photo, video }`.
- `photo` 있으면 원형 프레임에 사진, 없으면 실루엣 플레이스홀더
- `video` 있으면 카드 전체가 링크(사진 클릭 → 영상), 없으면 "영상 준비중"
- `YT_CHANNEL` 채우면 하단 노트가 채널 링크로 바뀜

## 사진 넣기 — `PHOTOS` 객체 (스크립트 최상단)
```js
var PHOTOS = {
  'village' : 'photos/village.jpg',
  'day1-1'  : 'photos/day1-1.jpg', 'day1-2': '...', 'day1-3': '...',
  'day2-1'  : '...', 'day2-2': '...', 'day2-3': '...',
  'team-1'..'team-4', 'glance-1'..'glance-3', 'cover', 'credits'
};
```
- `photos/` 폴더를 만들고 파일을 넣은 뒤 위 객체에 `키: '경로'` 추가 → 해당 자리의 "사진 자리" 플레이스홀더가 실제 `<img>`로 교체됨. 키는 각 `<figure class="snap" data-photo="...">` 에 이미 박혀 있음.
- 사진은 CSS `filter`로 따뜻한 톤 보정(`sepia .12 / saturate .9 / contrast .96`).
- 폴라로이드 프레임·마스킹테이프·회전은 자동.

## 남은 TODO
- **"마을 이야기" 페이지**: `STORIES` 배열 + `YT_CHANNEL`.
  ```js
  var STORIES = [
    { name: '김○○ 어르신', photo: 'photos/story-1.jpg', video: 'https://youtu.be/xxxx' },
    ...
  ];
  ```
  `photo` 있으면 인물 사진, 없으면 플레이스홀더. `video` 있으면 카드가 영상 링크 + "▶ 영상" 뱃지.
- 실제 사진 파일 (위 PHOTOS / STORIES.photo)
- 크레딧 페이지 인스타그램 핸들 확인(@smu_thewings)

## 참고 — 확정된 실제 데이터
참여 인원 19명(1365 등록 학생 18명 + 담당교직원 1명), 수혜자 22명, 활동 2일, 진행 프로그램 6개, 총 활동시간 152h, 전공연계 4개팀.

## 디자인 방향
"다이어리/일기 감성" — 우표 모양 스캘럽 카드, 그린 톤 컬러블록, 손그림 일러스트, 통통한 손글씨 느낌 폰트. 참고 이미지(제주·중국 샤오홍슈 스타일 다이어리 꾸미기 레퍼런스) 기반으로 리스킨한 버전입니다.
