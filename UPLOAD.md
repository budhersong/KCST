# budhersong/KCST 업로드 안내 — 2026-11-09 기준

이 폴더는 **저장소 루트와 1:1로 맞춘 완전한 사본**입니다.
폴더 안의 파일을 저장소 루트에 그대로 덮어쓰면 사이트가 현재 디자인과 완전히 같아집니다.

저장소 최신 커밋은 `189a4b6` (2026-10-30) 이고, 그 이후 이 프로젝트에서 바뀐 내용이 모두 반영되어 있습니다.

---

## 1. 이 폴더에 담긴 것

```
github_sync/
├── index.html          32,534 B   메인 페이지
├── kcst.css            21,929 B   전체 스타일
├── llms.txt             2,732 B   AI 검색용 요약
├── robots.txt             341 B   (저장소 사본과 동일 — 올리지 않아도 됨)
├── sitemap.xml            265 B   lastmod 2026-11-09 로 갱신
├── README.md                       배포 문서 (현재 구성 반영)
├── UPLOAD.md                       이 문서
└── assets/                        15개 — 페이지가 참조하는 전부
    ├── 로고 4종 · 심볼 · 파비콘 2종 · og-image
    ├── popup-poster-2026.png      708,539 B
    └── partner-*.png 6종
```

---

## 2. 무엇이 달라졌나

### 문구 — 후원기관

| 위치 | 이전 | 현재 |
|---|---|---|
| 섹션 영문 라벨 | `PARTNERS` | **`SPONSORS`** |
| 섹션 제목 | `함께하는 기관` | **`후원기관`** |
| 설명 | `창립대회와 학회 활동을 함께하는 기관입니다.` | **`창립대회와 학술사업을 후원해 주시는 기관입니다.`** |
| 하단 안내 | `후원과 협력에 관심 있는 기관은 …` | **`기관 후원과 공동 사업에 관심 있는 기관은 kcst026@gmail.com으로 연락 주세요.`** |
| 헤더·푸터 메뉴 | `함께하는 기관` | **`후원기관`** |

앵커 id `#partners` 와 파일명 `partner-*.png` 는 기존 링크·경로 호환을 위해 그대로 두었습니다.

### 그 밖에 저장소와 달라진 파일

| 파일 | 내용 |
|---|---|
| `index.html` | 후원기관 구역 추가, FAQ 번호 04→05, 포스터 `?v=4`, 스타일시트 `?v=9`, 회비 안내·신청 버튼 반영 |
| `kcst.css` | `.pt*` 후원기관 로고 밴드, 회비 패널·신청 버튼 스타일 |
| `llms.txt` | `## 후원기관` 항목과 후원 문의 문구 |
| `sitemap.xml` | `<lastmod>` 2026-10-21 → **2026-11-09** |
| `README.md` | 배포 문서를 현재 페이지 구성에 맞게 다시 작성 |
| `assets/` | 포스터 교체 + 후원기관 로고 6종 신규 |

---

## 3. 올리는 방법

### 방법 A — GitHub 웹에서 끌어다 놓기 (가장 간단, 권장)

1. https://github.com/budhersong/KCST 열기
2. **Add file → Upload files**
3. 이 폴더의 `index.html` · `kcst.css` · `llms.txt` · `sitemap.xml` · `README.md` 를 끌어다 놓기
4. **Commit changes** — 메시지 예: `후원기관 명칭·문구 반영, 포스터·로고 갱신`
5. 다시 **Add file → Upload files** → 이 폴더의 `assets` 폴더를 통째로 끌어다 놓기 (15개가 함께 올라갑니다) → **Commit changes**

### 방법 B — git (폴더 전체를 한 번에)

```bash
git clone https://github.com/budhersong/KCST.git
cd KCST
# 이 폴더(github_sync)의 내용을 저장소 루트에 그대로 복사
cp -R <이 폴더 경로>/* .
git add -A
git commit -m "후원기관 명칭·문구 반영, 포스터·로고 갱신, sitemap lastmod"
git push
```

### 방법 C — GitHub Desktop

이 폴더의 파일을 저장소 루트로 복사 → 변경 목록 확인 → Commit → Push.

---

## 4. 지워도 되는 파일 (선택)

페이지가 더 이상 참조하지 않습니다. 지워도 사이트는 정상 동작하고, 저장소가 약 3.9 MB 가벼워집니다.

| 경로 | 크기 |
|---|---|
| `assets/popup-membership-poster.png` | 3,206,399 B |
| `assets/poster-founding-conference.png` | 698,022 B |

이 폴더에는 두 파일을 **넣지 않았습니다**. 저장소에서 지우지 않아도 무해합니다.

---

## 5. 올린 뒤 확인

**파일 크기부터** 맞춰 보세요. GitHub 파일 화면에 바이트 단위로 표시됩니다.

| 파일 | 기준 크기 |
|---|---|
| `index.html` | 32,534 bytes |
| `kcst.css` | 21,929 bytes |
| `llms.txt` | 2,732 bytes |
| `assets/popup-poster-2026.png` | 708,539 bytes |
| `assets/partner-*.png` | 6개, 각 6.8–10.2 KB |

그다음 https://budhersong.github.io/KCST/ 에서:

1. 로드맵 아래 **‘후원기관’** 구역에 로고 6개가 한 줄로 표시되는지
2. 라벨이 **`SPONSORS` / `후원기관`**, 설명이 **‘창립대회와 학술사업을 후원해 주시는 기관입니다.’** 인지
3. 하단 안내가 **‘기관 후원과 공동 사업에 관심 있는 기관은 kcst026@gmail.com으로 연락 주세요.’** 인지
4. 헤더·푸터 메뉴에 **‘후원기관’** 이 있는지
5. 히어로 **‘창립대회 11.17–18 포스터 →’** 를 눌러 팝업이 **제1회 심포지엄·정기 총회 포스터**로 뜨는지
6. 좁은 화면에서 후원기관 로고가 3개/2개씩 줄바꿈되는지

> 예전 화면이 그대로 보이면 브라우저 캐시입니다. 강력 새로고침(Ctrl/Cmd + Shift + R). GitHub Pages 반영에 1–2분 걸릴 수 있습니다.

---

## 6. 주의 — `.nojekyll`

저장소 루트의 `.nojekyll`(0바이트)은 **지우지 마세요.** 없으면 GitHub Pages가 Jekyll을 돌려 `index.html` 대신 `README.md` 를 홈페이지로 렌더링합니다.
이번에 `README.md` 를 교체했으니, 홈페이지에 문서가 보인다면 이 파일이 사라진 것이니 루트에 빈 `.nojekyll` 을 다시 만들어 주세요.
