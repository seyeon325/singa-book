# 신가1리 이야기

2026년 여름, 상명대학교 그린나래가 충남 천안 신가1리에서 보낸 1박 2일의 기록을
책장을 넘기듯 읽는 웹 필드저널입니다.

- 단일 파일: [`index.html`](index.html) — 빌드 없음, 더블클릭으로 바로 열림
- 외부 의존성: Google Fonts 한 곳
- 자세한 구조·수정 방법: [`HANDOFF.md`](HANDOFF.md)

## 로컬에서 보기

`index.html`을 브라우저로 열면 됩니다. 특정 페이지로 바로 가려면 주소 끝에
`#p3` 처럼 페이지 번호를 붙입니다.

## GitHub Pages 배포

```bash
# 1. GitHub에서 빈 저장소를 하나 만든 뒤 (README/라이선스 체크 해제)
git remote add origin https://github.com/<사용자명>/<저장소명>.git
git branch -M main
git push -u origin main

# 2. GitHub 저장소 → Settings → Pages
#    Source: "Deploy from a branch" / Branch: main / 폴더: / (root) → Save
```

몇 분 뒤 `https://<사용자명>.github.io/<저장소명>/` 에서 열립니다.

Netlify·Vercel은 이 저장소를 연결하고 빌드 명령 없이 루트를 그대로 배포하면 됩니다.
