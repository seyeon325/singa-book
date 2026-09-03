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
- 접근성: 상단 진행바(`#progressBar`), 스크린리더용 `aria-live` 영역(`#liveRegion`)이 페이지 전환을 읽어줌
- 760px 이상: 데스크탑 펼침책(스프레드) 뷰 / 미만: 모바일 단일 페이지 뷰로 CSS 미디어쿼리 자동 전환

## 남은 TODO (사용자 쪽에서 채워 넣을 부분)
- **"마을 이야기" 페이지**: 스크립트 최상단 `STORIES` 배열과 `YT_CHANNEL` 상수만 수정하면 됩니다.
  ```js
  var YT_CHANNEL = 'https://www.youtube.com/@그린나래채널';  // 비우면 안내 문구만 표시
  var STORIES = [
    { name: '김○○ 어르신', note: '한 줄 소개(선택)', video: 'https://youtu.be/xxxx' },
    ...
  ];
  ```
  `video`가 있으면 카드가 영상 링크로, 없으면 "영상 준비중"으로 표시됩니다. 배열 길이는 자유(2열 그리드).
- 어르신 실제 프로필 사진은 아직 미반영 — 넣으려면 카드 아이콘(`person` SVG) 자리에 `<img>` 교체 필요 (요청 시 작업)
- 크레딧 페이지 인스타그램 핸들 확인(@smu_thewings)

## 참고 — 확정된 실제 데이터
참여 인원 19명(1365 등록 학생 18명 + 담당교직원 1명), 수혜자 22명, 활동 2일, 진행 프로그램 6개, 총 활동시간 152h, 전공연계 4개팀.

## 디자인 방향
"다이어리/일기 감성" — 우표 모양 스캘럽 카드, 그린 톤 컬러블록, 손그림 일러스트, 통통한 손글씨 느낌 폰트. 참고 이미지(제주·중국 샤오홍슈 스타일 다이어리 꾸미기 레퍼런스) 기반으로 리스킨한 버전입니다.
