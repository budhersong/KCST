# KCST 랜딩페이지 — 배포 패키지

이 폴더는 **그대로 웹 서버에 올리면 동작하는** 정적 사이트입니다. 빌드 과정이 없습니다.

```
deploy/
├── index.html              메인 페이지 (SEO 메타 + 구조화 데이터 + 스크립트 포함)
├── kcst.css                전체 스타일
├── robots.txt              검색로봇 정책
├── sitemap.xml             사이트맵
├── llms.txt                AI 검색용 요약
└── assets/
    ├── kcst-symbol.png     심볼 (회장 소개 블록, 파비콘)
    ├── apple-touch-icon.png
    ├── favicon-512.png
    ├── logo-h-dark.png     가로형 · 반전 (어두운 배경용)
    ├── logo-h-light.png    가로형 · 기본 (밝은 배경용)
    ├── logo-v-dark.png     세로형 · 반전
    ├── logo-v-light.png    세로형 · 기본
    ├── og-image.png        공유 미리보기 이미지 1200×630
    └── poster-founding-conference.png  창립대회 포스터 (팝업에서 표시)
```

> `index.html.srcmap.json` 은 편집기 부속 파일입니다. 업로드해도 무해하지만, 올리지 않아도 됩니다.

---

## 배포 방법

### 방법 A — Genspark Code (권장)

이 프로젝트에서 코드 편집·배포까지 이어서 진행할 수 있습니다. 저장소로 넘기면 도메인 연결과 자동 배포까지 한 흐름으로 처리됩니다.

### 방법 B — 정적 호스팅 (Netlify · Vercel · Cloudflare Pages · GitHub Pages)

1. `deploy/` 폴더 **안의 내용물**을 저장소 루트에 그대로 넣습니다.
   (`deploy` 폴더 자체가 아니라 그 안의 파일들이 사이트 최상위에 오도록)
2. 빌드 명령 없음 / 출력 디렉터리 `.` (또는 루트)로 설정합니다.
3. `robots.txt` · `sitemap.xml` · `llms.txt` 가 **사이트 최상위**에 있는지 확인합니다.
   (`https://도메인/robots.txt` 로 접속해 내용이 보여야 정상입니다.)

### 방법 C — 일반 웹호스팅 (cafe24 · 가비아 등)

FTP로 `deploy/` 안의 내용물을 웹 루트(`www` 또는 `public_html`)에 업로드합니다. 서버가 Apache라면 아래 `.htaccess` 를 추가하면 주소 끝 슬래시와 캐시 설정이 정리됩니다.

```apache
# .htaccess
Options -MultiViews
DirectoryIndex index.html

# 캐시: CSS·JS·이미지는 오래, HTML은 짧게
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType text/css "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType text/html "access plus 1 hour"
</IfModule>
```

---

## 배포 직후 반드시 할 일

### 1. 도메인 일괄 치환

지금 `index.html` 과 `sitemap.xml`·`robots.txt` 에는 임시 주소 `https://www.kcst.or.kr/` 가 들어 있습니다. 실제 도메인으로 **전부** 바꿔야 합니다.

`index.html` 안에서 임시 주소가 쓰인 곳:

| 위치 | 개수 | 용도 |
|---|---|---|
| `<link rel="canonical">` | 1 | 대표 주소 |
| `<link rel="alternate" hreflang="ko">` | 1 | 언어별 주소 |
| `og:url` · `og:image` · `twitter:image` | 3 | 공유 미리보기 |
| JSON-LD `@id` (`#org`, `#president`, `#website`) | 6 | 구조화 데이터 식별자 |
| JSON-LD `url` · `logo` · `image` | 5 | 학회·웹사이트 정보 |

`@id` 값들은 서로를 참조하므로 **도메인만 바꾸고 나머지 형식은 그대로** 두면 됩니다.

또한 `sitemap.xml` 의 `<loc>` 와 `robots.txt` 마지막 줄의 `Sitemap:` 주소도 함께 바꿔 주세요.

치환 예시 (리눅스/맥):
```bash
cd deploy
sed -i '' 's|https://www\.kcst\.or\.kr/|https://실제도메인/|g' index.html sitemap.xml robots.txt
```
(리눅스는 `sed -i` , 맥은 `sed -i ''`)

### 2. 검색엔진 소유 확인 코드 입력

`index.html` 상단에 자리만 만들어 둔 두 줄을 실제 발급 코드로 채웁니다.

```html
<meta name="naver-site-verification" content="NAVER_VERIFICATION_CODE">
<meta name="google-site-verification" content="GOOGLE_VERIFICATION_CODE">
```

- **네이버**: 서치어드바이저 → 사이트 등록 → HTML 태그 방식
- **구글**: Search Console → 속성 추가 → HTML 태그 방식

발급받은 코드를 `content="..."` 안에 그대로 붙여 넣고 다시 배포합니다.

### 3. 검색엔진에 사이트맵 제출

- 네이버 서치어드바이저 → 요청 → **사이트맵 제출** → `sitemap.xml`
- 구글 Search Console → 색인 → **Sitemaps** → `sitemap.xml`
- (선택) 빙 웹마스터 도구에도 같은 주소를 제출

제출 후 며칠 안에 네이버·구글 검색결과에 노출되기 시작합니다.

### 4. 남은 임시값 5가지

| 항목 | 현재 상태 | 필요한 것 |
|---|---|---|
| 창립대회 장소 | 구조화 데이터에 `장소 추후 공지` | 확정된 행사장 이름과 주소 |
| 입회원서 | 이메일로 요청하도록 안내 | 신청 폼 주소 (구글폼 등) — 만들면 FAQ 4번 문구와 버튼 교체 |
| 연락 이메일 | `leesh45337@gmail.com` | 학회 공식 주소가 따로 있다면 FAQ 안내·푸터 두 곳 동시 교체 || 공식 로고 원본 | 심볼·파비콘은 로고 이미지에서 잘라 만든 파일 | BI 가이드라인의 공식 PNG 600dpi / ICO·SVG 배포본으로 교체 |
| 포스터 해상도 | 가로 683px (682KB) | 인쇄용 원본에서 1400px 이상으로 다시 받아 교체 (선택) |

---

## 확인 체크리스트

배포 후 아래를 직접 열어 확인하세요.

- [ ] `https://도메인/` — 페이지가 정상 표시, 디자인이 깨지지 않음
- [ ] `https://도메인/robots.txt` — 내용이 보임
- [ ] `https://도메인/sitemap.xml` — 내용이 보임
- [ ] `https://도메인/llms.txt` — 내용이 보임
- [ ] **모바일**에서 열어 가로 스크롤이 생기지 않음
- [ ] 모바일에서 우측 상단 버튼을 눌러 메뉴가 열리고 닫힘
- [ ] FAQ 항목을 눌러 펼쳐지고 접힘
- [ ] 푸터의 이메일을 누르면 메일 앱이 열림
- [ ] **히어로의 ‘창립대회 11.17–18 포스터 →’ 버튼을 누르면 포스터 팝업이 뜨고**, X 버튼·바깥 어두운 영역·ESC 키로 닫힘
- [ ] 팝업이 열렸을 때 뒤 배경이 스크롤되지 않고, 닫으면 원래 위치로 돌아옴
- [ ] 모바일에서 팝업이 화면에 꽉 차서 포스터가 잘 보임
- [ ] 카카오톡·네이버에 주소를 붙였을 때 미리보기 카드(제목·이미지)가 뜸
- [ ] 검색창에 `한국과학기술문화소통학회` 로 검색했을 때 노출 (색인까지 수일 소요)

### 구조화 데이터 검증 (권장)

배포 후 아래 도구에 주소를 넣으면 구조화 데이터가 올바른지 검사해 줍니다.

- 구글 리치 결과 테스트 — `search.google.com/test/rich-results`
- Schema.org 검증기 — `validator.schema.org`

학회·회장·행사·FAQ·웹사이트 정보가 각각 인식되면 정상입니다.

---

## 수정할 때 주의할 점

1. **CSS 주소의 버전 표시를 유지하세요.** `index.html` 에 `kcst.css?v=4` 로 되어 있는데, 이 숫자를 올려야 방문자 브라우저가 새 스타일을 즉시 받습니다.

2. **FAQ 문구와 구조화 데이터는 한 쌍입니다.** 화면의 FAQ 8개와 `index.html` 의 `"@type":"FAQPage"` 안 답변이 같은 내용입니다. 한쪽만 고치면 구글이 불일치로 표시하므로 **반드시 둘 다** 고쳐야 합니다.

3. **한국어 줄바꿈 설정을 지우지 마세요.** `kcst.css` 의 `body` 에 있는 `word-break: keep-all` 이 빠지면 한글이 단어 중간에서 끊겨 보입니다.

4. **히어로 영역의 `overflow:hidden` 을 지우지 마세요.** 모바일에서 장식 원호가 화면 밖으로 나가 가로 스크롤이 생기는 것을 막고 있습니다.

5. **이메일 링크는 일부러 나눠 쓴 형태입니다.** `index.html` 의 이메일 두 곳은 이렇게 되어 있습니다.

   ```html
   <a class="mail" data-u="leesh45337" data-d="gmail.com">leesh45337&#64;gmail&#46;com</a>
   ```

   이렇게 쓴 이유는, 저장·전송 과정에서 이메일 주소가 자동으로 난독화 코드(`/cdn-cgi/...`)로 바뀌어 들어가 메일 링크가 깨지는 일이 있었기 때문입니다. 주소를 두 조각으로 나눠 두면 그 변환이 일어나지 않고, 페이지가 열릴 때 스크립트가 `mailto:` 주소를 붙여 줍니다.

   - 주소를 바꾸려면 `data-u`(골뱅이 앞)와 `data-d`(골뱅이 뒤) 두 값과, 화면에 보이는 글자를 함께 고쳐야 합니다.
   - 주소가 바뀌면 FAQ 안내와 푸터 **두 곳 모두** 수정하세요.
   - 만약 다른 호스팅으로 옮겨 이 변환이 일어나지 않는다면, 그냥 `<a href="mailto:주소">주소</a>` 로 되돌려도 됩니다. 오히려 그 편이 더 단순합니다.

6. **행사 날짜를 바꾸려면 두 곳을 고쳐야 합니다.** `kcst.css` 가 아니라 `index.html` 하단 스크립트의 디데이 계산(`2026-11-17`, `2026-11-19`)과 구조화 데이터의 `startDate`·`endDate` 입니다.

7. **포스터 팝업 이미지를 교체하거나 추가하세요.** 창립대회 포스터는 `assets/poster-founding-conference.png` 이고, 히어로의 **‘창립대회 11.17–18 포스터 →’** 버튼을 누르면 뜨는 팝업에서 표시됩니다. 팝업 안에 ‘이미지 저장’ 링크도 있어 방문자가 내려받을 수 있습니다.

   - 포스터를 바꾸려면 **같은 파일명으로 덮어쓰기**만 하면 됩니다. 파일명이 바뀌면 `index.html` 의 두 곳(`<img src>` 와 ‘이미지 저장’ 링크)도 함께 고쳐야 합니다.
   - 현재 파일은 가로 683px입니다. 팝업은 최대 720px 폭이라 지금은 충분하지만, 더 선명하게 보이게 하려면 인쇄용 원본에서 **2배 크기(약 1400px 폭) 이상**으로 다시 받아 교체하시면 좋습니다.
   - 이미지 파일이 682KB로 페이지에서 가장 무거운 요소입니다. 팝업을 열 때만 불러오는 방식(지연 로딩)으로 바꾸면 첫 화면이 더 빨라집니다.

---

## 참고: 개발자용 상세 명세

화면 구성·디자인 토큰·동작 방식까지 수치로 정리한 문서는 별도 핸드오프 패키지(`design_handoff_kcst_landing_page/README.md`)에 있습니다. 프레임워크(React·Vue 등)로 다시 구현하거나 디자이너와 협업할 때 참고하세요.
