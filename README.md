# KCST 랜딩페이지 — 한국과학기술문화소통학회

학회 홍보용 단일 페이지. **빌드 과정이 없는 정적 사이트**입니다. 저장소 루트에 있는 파일이 그대로 웹에 올라갑니다.

> **현재 배포 주소:** https://budhersong.github.io/KCST/

```
/
├── index.html              메인 페이지 (SEO 메타 + 구조화 데이터 + 스크립트 포함)
├── kcst.css                전체 스타일
├── robots.txt              검색로봇 정책
├── sitemap.xml             사이트맵
├── llms.txt                AI 검색용 요약
├── .nojekyll               (지우지 마세요 — 아래 설명)
└── assets/
    ├── kcst-symbol.png     심볼 (회장 소개 블록, 파비콘)
    ├── apple-touch-icon.png
    ├── favicon-512.png
    ├── logo-h-dark.png     가로형 · 반전 (어두운 배경용)
    ├── logo-h-light.png    가로형 · 기본 (밝은 배경용)
    ├── logo-v-dark.png     세로형 · 반전
    ├── logo-v-light.png    세로형 · 기본 (구조화 데이터 logo)
    ├── og-image.png        공유 미리보기 이미지 1200×630
    ├── popup-poster-2026.png      제1회 KCST 심포지엄 · 정기 총회 포스터 (팝업에 표시)
    └── partner-*.png              후원기관 로고 6종
                                   SciCom Zhejiang · exco · KENTECH
                                   KARMA · 전자신문 · HelloDoo
```

> `.nojekyll` 은 **지우지 마세요.** 이 파일이 없으면 GitHub Pages가 Jekyll을 돌려 `index.html` 대신 `README.md`를 홈페이지로 렌더링합니다.
>
> `index.html.srcmap.json` 은 편집기 부속 파일입니다. 저장소에 두어도 무해하지만, 올리지 않아도 됩니다.

---

## 페이지 구성

상단 고정 헤더 / 히어로(3줄 제목 + 통계 3개 + 포스터 팝업 버튼) / **학회 소개**(가치 카드 3개, 참여 대상, 회장 소개) / **핵심 사업**(카드 6개) / **로드맵**(4단계) / **후원기관**(로고 6개) / **FAQ**(8개, 회원가입 항목에 회비 안내) / 푸터.

영문 라벨은 섹션별로 `ABOUT KCST` · `PROGRAMS` · `ROADMAP` · `SPONSORS` · `FAQ` 입니다.

---

## 배포 직후 반드시 할 일

### 1. 도메인 일괄 치환

`index.html` · `sitemap.xml` · `robots.txt` 에 임시 주소 `https://www.kcst.or.kr/` 가 들어 있습니다. 실제 도메인으로 **전부** 바꿔야 합니다.

`index.html` 안에서 임시 주소가 쓰인 곳 (17줄):

| 위치 | 개수 | 용도 |
|---|---|---|
| 파일 상단 HTML 주석 | 1 | 치환 안내 메모 |
| `<link rel="canonical">` | 1 | 대표 주소 |
| `<link rel="alternate" hreflang="ko">` | 1 | 언어별 주소 |
| `og:url` · `og:image` · `twitter:image` | 3 | 공유 미리보기 |
| JSON-LD `@id` (정의 3 + 참조 3) | 6 | 구조화 데이터 식별자 |
| JSON-LD `url` · `logo` · `image` | 5 | 학회·웹사이트 정보 |

`@id` 값들은 서로를 참조하므로 **도메인만 바꾸고 나머지 형식은 그대로** 두면 됩니다. `sitemap.xml` 의 `<loc>` 와 `robots.txt` 마지막 줄의 `Sitemap:` 주소도 함께 바꿔 주세요.

```bash
sed -i '' 's|https://www\.kcst\.or\.kr/|https://실제도메인/|g' index.html sitemap.xml robots.txt
# 리눅스는 sed -i , 맥은 sed -i ''
```

### 2. 검색엔진 소유 확인 코드 입력

```html
<meta name="naver-site-verification" content="NAVER_VERIFICATION_CODE">
<meta name="google-site-verification" content="GOOGLE_VERIFICATION_CODE">
```

- **네이버** 서치어드바이저 → 사이트 등록 → HTML 태그 방식
- **구글** Search Console → 속성 추가 → HTML 태그 방식

### 3. 검색엔진에 사이트맵 제출

- 네이버 서치어드바이저 → 요청 → **사이트맵 제출** → `sitemap.xml`
- 구글 Search Console → 색인 → **Sitemaps** → `sitemap.xml`

### 4. 남은 임시값

| 항목 | 현재 상태 | 필요한 것 |
|---|---|---|
| 창립대회 장소 | 구조화 데이터에 `장소 추후 공지` (포스터에는 한국과학기술회관으로 인쇄됨) | 학회 확인 후 확정된 행사장 이름과 주소를 구조화 데이터·FAQ에 반영 |
| 공식 로고 원본 | 심볼·파비콘은 로고 이미지에서 잘라 만든 파일 | BI 가이드라인의 공식 PNG 600dpi / ICO·SVG 배포본으로 교체 |
| 회비 구분 | 패널은 임원 / 정회원 / 대학원생 / 대학생 이하, FAQ 본문은 정회원 / 학생회원 / 단체회원 | 학회 실제 기준으로 두 곳 통일 (단체회원 회비 미정) |
| 포스터 해상도 | 가로 682px (692KB) | 인쇄용 원본에서 1400px 이상으로 다시 받아 교체 (선택) |

---

## 확인 체크리스트

- [ ] `https://도메인/` — 페이지가 정상 표시, 디자인이 깨지지 않음
- [ ] `https://도메인/robots.txt` · `/sitemap.xml` · `/llms.txt` — 내용이 보임
- [ ] **모바일**에서 가로 스크롤이 생기지 않음
- [ ] 모바일에서 우측 상단 버튼으로 메뉴가 열리고 닫힘
- [ ] FAQ 항목이 펼쳐지고 접힘, 회원가입 항목에 회비 안내와 신청 버튼이 보임
- [ ] **로드맵 아래 ‘후원기관’ 구역에 로고 6개가 한 줄**로 보이고, 좁은 화면에서 3개/2개씩 줄바꿈됨
- [ ] **히어로의 ‘창립대회 11.17–18 포스터 →’** 를 누르면 포스터 팝업이 뜨고, X · 바깥 영역 · ESC 로 닫힘
- [ ] 팝업이 열렸을 때 뒤 배경이 스크롤되지 않고, 닫으면 원래 위치로 돌아옴
- [ ] 팝업 상단 주황색 **‘회원 가입 신청’** 을 누르면 구글 폼이 새 탭으로 열림
- [ ] 카카오톡·네이버에 주소를 붙였을 때 미리보기 카드가 뜸

### 구조화 데이터 검증 (권장)

- 구글 리치 결과 테스트 — `search.google.com/test/rich-results`
- Schema.org 검증기 — `validator.schema.org`

학회·회장·행사·FAQ·웹사이트가 각각 인식되면 정상입니다.

---

## 수정할 때 주의할 점

1. **CSS 주소의 버전 표시를 유지하세요.** `index.html` 에는 `kcst.css?v=9` 로 되어 있습니다. 스타일을 바꾸면 이 숫자를 올려야 방문자 브라우저가 새 CSS를 즉시 받습니다.
2. **FAQ 문구와 구조화 데이터는 한 쌍입니다.** 화면의 FAQ 8개와 `"@type":"FAQPage"` 안 답변은 같은 내용입니다. 한쪽만 고치면 구글이 불일치로 표시하므로 **반드시 둘 다** 고쳐야 합니다. 회비 금액과 계좌번호도 마찬가지입니다.
3. **한국어 줄바꿈 설정을 지우지 마세요.** `kcst.css` 의 `body` 에 있는 `word-break: keep-all` 이 빠지면 한글이 단어 중간에서 끊겨 보입니다.
4. **히어로의 `overflow:hidden` 을 지우지 마세요.** 모바일에서 장식 원호가 화면 밖으로 나가 가로 스크롤이 생기는 것을 막고 있습니다.
5. **이메일 링크는 일부러 나눠 쓴 형태입니다.**

   ```html
   <a class="mail" data-u="kcst026" data-d="gmail.com">kcst026&#64;gmail&#46;com</a>
   ```

   저장·전송 과정에서 주소가 자동 난독화 코드(`/cdn-cgi/...`)로 바뀌어 메일 링크가 깨진 적이 있어, 주소를 두 조각으로 나눠 두었습니다. 페이지가 열릴 때 스크립트가 `mailto:` 를 붙입니다. 주소를 바꾸려면 `data-u` · `data-d` 와 화면에 보이는 글자를 함께 고치고, FAQ 안내와 푸터 **두 곳 모두** 수정하세요.

6. **행사 날짜를 바꾸려면 두 곳**을 고칩니다. `index.html` 하단 스크립트의 디데이 계산(`2026-11-17`, `2026-11-19`)과 구조화 데이터의 `startDate`·`endDate` 입니다.

7. **포스터 팝업 이미지**는 `assets/popup-poster-2026.png` 이고, 히어로 버튼을 누르면 뜹니다. 같은 파일명으로 덮어쓰면 되고, 방문자에게 예전 포스터가 계속 보이면 `index.html` 두 곳의 `?v=4` 를 `?v=5` 로 올리세요.

8. **후원기관 로고는 원본 그대로** 쓰세요. 비율 유지, 높이 44px 이하, 색 변형·크롭 금지. 로고를 추가·삭제하면 `#partners` 의 `<ul>` 과 `llms.txt` 의 ‘후원기관’ 항목을 함께 수정합니다. `width`/`height` 속성은 지우지 마세요(로고가 늦게 로드될 때 구역이 흔들립니다).

---

## 참고: 개발자용 상세 명세

화면 구성·디자인 토큰·동작 방식까지 수치로 정리한 문서는 별도 핸드오프 패키지(`design_handoff_kcst_landing_page/README.md`)에 있습니다. 프레임워크(React·Vue 등)로 다시 구현하거나 디자이너와 협업할 때 참고하세요.
