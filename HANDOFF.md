# 신가1리 이야기 — 웹사이트 인수인계 노트

## 파일 구성
- `index.html` — 완성된 단일 HTML 파일 (원본 `singa-book.html`을 리네임). 인라인 CSS/JS만 사용하고 외부 의존성은 Google Fonts(Jua, Gowun Dodum, Gaegu) 하나뿐입니다.
- 빌드 과정 없음 (React·번들러·npm 없음). 더블클릭해서 브라우저로 바로 열리고, `file://` 경로에서도 정상 동작합니다.
- git 저장소로 관리됩니다 (`~/source/repos/singa-book`, GitHub: `seyeon325/singa-book`).

## 배포
- **공개 주소: https://seyeon325.github.io/singa-book/**
- GitHub Pages, `main` 브랜치 루트에서 자동 배포. `main`에 push하면 1~2분 뒤 반영됩니다.
- 수정 → 배포: `git add -A && git commit -m "..." && git push`

## Claude Code에서 이어가는 방법
1. `singa-book.html`을 원하는 프로젝트 폴더(또는 새 git 저장소)에 넣습니다.
2. 그 폴더에서 `claude` 실행 후 "이 파일 열어서 ○○ 부분 고쳐줘" 식으로 요청하면 됩니다. 파일 하나짜리라 별도 설정 없이 바로 인식합니다.
3. 배포까지 하려면 Claude Code에게 "GitHub Pages(혹은 Vercel/Netlify)로 배포해줘"라고 하면 됩니다.

## 디자인 토큰 (`:root` CSS 변수)
| 변수 | 값 | 용도 |
|---|---|---|
| `--leaf` | #2FAE5C | 메인 그린 |
| `--leaf-deep` | #1C7A3E | 진한 그린(스탬프 텍스트 등) |
| `--bg` | #F3EFDF | 책 바깥 배경(크림) |
| `--paper` / `--paper-shade` | #FFFDF6 / #EFE8D2 | 페이지 종이색 |
| `--ink` / `--ink-soft` | #20301C / #63715A | 본문 텍스트 |
| `--butter` `--sky` `--blush` `--sage` | 파스텔 4색 | 카드/태그 컬러 로테이션 |

폰트: 제목 `Jua`, 본문 `Gowun Dodum`, 손글씨 포인트 `Gaegu`.

## 페이지 구성 (총 9장, 책장 넘김)
0. 표지 → 1. 프롤로그(목차, 클릭 시 해당 페이지로 자동 이동) → 2. 마을 소개 → 3. 한눈에 보는 활동(통계) → 4. Day1 → 5. Day2 → 6. 네 팀의 역할 → 7. 마을 이야기(어르신 카드) → 8. 크레딧/마무리

## 인터랙션 구조 (스크립트 하단 IIFE)
- `doAdvance()` / `doRetreat()` — 실제 페이지 상태 변경
- `layout()` — z-index 스택 관리
- `goTo(target)` — 목차 클릭 시 자동 넘김(애니메이션, 폴링 방식)
- `jump(target)` — 애니메이션 없이 즉시 이동 (URL 해시 복원·Home/End 키)
- `syncHash()` / `pageFromHash()` — URL 해시(`#p3`)와 현재 페이지 동기화. 특정 페이지 링크 공유·새로고침 복원 가능
- `onDown/onMove/onUp` — 마우스·터치 드래그로 페이지 넘기기
- 키보드: ←/→, PageUp/PageDown, Space(Shift+Space 뒤로), Home, End
- 모바일: 화면 왼쪽 30% 탭 = 뒤로, 그 외 탭 = 다음. 좌우 스와이프로도 넘김(스와이프 방향이 방향 결정). 세로 드래그는 본문 스크롤로 통과
- 데스크탑: 펼침책의 좌/우 절반을 잡고 드래그
- 접근성: 상단 진행바(`#progressBar`), 스크린리더용 `aria-live` 영역(`#liveRegion`)
- 760px 이상: 데스크탑 펼침책(스프레드) 뷰 / 미만: 모바일 단일 페이지 뷰

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
