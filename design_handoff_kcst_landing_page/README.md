# Handoff: KCST 홍보용 랜딩페이지 (한국과학기술문화소통학회)

## Overview

A single-page responsive marketing/landing page for **한국과학기술문화소통학회 (KCST — The Korean Society for the Public Communication of Science and Technology)**, a Korean academic society founded 2026 for the public communication of science and technology.

The page has two jobs:

1. **Explain the society to a first-time visitor** — who they are, what they do, where they are heading, and how to join.
2. **Be found and correctly summarized by search engines and AI answer engines** — Naver, Google, and LLM crawlers (GEO / generative engine optimization).

The audience is Korean. Everything is Korean-language copy, mobile-first, and designed to be readable on a phone at a café table as well as on a laptop.

**Deliberate constraint:** the page is intentionally *not* a transcription of the society's internal introduction deck. Earlier revisions pasted deck content (slide-by-slide internal statistics, org charts, bylaw clauses) and were rejected. The final structure is four public-facing sections only. Do not reintroduce internal-document content.

## About the Design Files

**The bundled files are design references created in HTML — prototypes showing the intended look, copy, and behavior. They are not production code to copy directly.**

The developer's task is to **recreate this design in the target codebase's existing environment** (React/Next.js, Vue/Nuxt, Astro, plain HTML templating, etc.) using that project's established patterns, component library, and build setup. If the project has no existing environment yet, a static-site setup (Astro or Next.js with static export) is the most appropriate choice, because this page has no server-side data requirements — its only dynamic behavior is a countdown, a mobile menu, and FAQ disclosure.

Whatever the framework, the following must be preserved verbatim:

- **All copy text** (Korean, exact wording, including intentional `<br>` line breaks — see "Copy with intentional line breaks" below)
- **All colors, type sizes, spacing, and radii** (see Design Tokens)
- **The SEO/GEO head markup and structured data** (see SEO & GEO Requirements) — this is a functional requirement, not decoration
- The four companion files (`robots.txt`, `sitemap.xml`, `llms.txt`) as real deployed files at the site root

## Fidelity

**High-fidelity (hifi).** This is a pixel-accurate mockup with final brand colors, typography, spacing, and interaction states. Recreate the UI pixel-perfectly using the codebase's existing libraries and patterns.

The color palette, typefaces, and logo geometry come from the society's official **BI 가이드라인 v1.0 (2026.10)** and are not open to improvisation. Details in Design Tokens.

---

## Screens / Views

This is one continuous scrolling page. There are six distinct visual regions, in DOM order.

### Global — Sticky Header

- **Purpose:** brand presence, section navigation, primary join CTA.
- **Layout:** `position: fixed; top:0; left:0; right:0; z-index:50; height:68px`, flex container, `align-items:center`. Inner `.wrap` is a flex row with `justify-content:space-between`.
- **Behavior — two visual states:**
  - **Transparent (top of page, `scrollY <= 40`):** transparent background, white/paper logo (`logo-h-dark.png` — light artwork), nav links `color: var(--paper)` at `opacity:.85`.
  - **Solid (`scrollY > 40`, class `is-solid`):** `background: rgba(244,245,242,.94)` (paper @ 94%), `backdrop-filter: blur(12px)`, `box-shadow: 0 1px 0 #DCE0DA`. Logo swaps to the navy version (`logo-h-light.png` — dark artwork); nav links become `var(--text)`.
  - The `is-solid` class also swaps the hamburger bars from paper to `var(--text)`.
- **Logo:** `height: 34px; width: auto`. Both logo variants are in the DOM; visibility is toggled by CSS class, not by JS swapping `src`.
- **Nav links** (desktop): `gap:28px`, `font-size:15px`, `font-weight:500`, `text-decoration:none`. Hover: `opacity:1`.
  - Labels and targets: 학회 소개 `#about` · 핵심 사업 `#programs` · 로드맵 `#roadmap` · FAQ `#faq`
  - Then a pill button: 회원 가입 → `#faq-join` (opens that FAQ item — see Interactions)
- **Mobile (≤860px):** the nav becomes a fixed dropdown panel under the header: `position:fixed; top:68px; left:0; right:0; background:var(--paper)`, links stacked full width with `border-bottom:1px solid var(--line)`, `font-size:17px`, `padding:14px 0`. Hidden state is `opacity:0; transform:translateY(-8px); pointer-events:none`; open state (`header.is-open`) is `opacity:1; transform:none; pointer-events:auto`. Transition `.25s`.
  - Hamburger button: `display:none` on desktop, `display:block` at ≤860px, `44px × 44px` (minimum touch target). Three 22px × 2px bars; when `is-open`, the middle bar fades and the outer two rotate ±45° into an X.
  - Button carries `aria-expanded` and an `aria-label` that toggles between "메뉴 열기" and "메뉴 닫기". Clicking any nav link closes the panel.

### 1 — Hero (`#top`, class `.hero`)

- **Purpose:** state the society's identity and positioning in one glance; offer two next actions.
- **Background:** `var(--navy)` `#14202E`, text `var(--paper)` `#F4F5F2`.
- **Padding:** `clamp(130px,18vw,190px) 0 0` (top padding clears the fixed 68px header). `overflow:hidden` — this clips the decorative ring on mobile and is load-bearing (see Responsive).
- **Layout:** two-column grid `.hero__grid`: `grid-template-columns: 1.15fr .85fr; gap:40px; align-items:center`.
- **Left column:**
  1. Eyebrow `.eyebrow` — text `2026 Founding · 사단법인`. Space Grotesk, 13px, `font-weight:500`, `letter-spacing:.16em`, uppercase, color `var(--mist)`. Preceded by a 8px circular dot in `var(--green)` (`::before`), `gap:10px`.
  2. H1 — `font-size: clamp(40px,6.4vw,84px)`, `font-weight:700`, `line-height:1.12`, `letter-spacing:-.035em`, `margin:22px 0 26px`. Three lines, explicit `<br>` between them, third line in `<em>` colored `var(--orange)`:
     ```
     과학소통에
     학술의 깊이를,
     한국에서 세계로
     ```
  3. `.hero__lead` — `font-size: clamp(17px,1.6vw,20px)`, `color:#C9D3DD`, `max-width:34em`. Begins with `<strong>한국과학기술문화소통학회(KCST)</strong>`.
  4. `.hero__en` — `The Korean Society for the Public Communication of Science and Technology`. Space Grotesk, 14px, `color: var(--slate)`, `margin-top:14px`. Has `lang="en"`.
  5. `.hero__cta` — flex row, `gap:12px`, `flex-wrap:wrap`, `margin-top:36px`. Two buttons:
     - Primary `.btn.btn--o`: `창립대회 11.17–18 →` → `#roadmap`
     - Secondary `.btn.btn--ghost`: `회원 가입 안내` → `#faq-join`
- **Right column — `.hero__art` (decorative, `aria-hidden="true"`)**
  A large abstract rendering of the KCST symbol, built entirely in CSS. `aspect-ratio:1`, `width:100%`, `max-width:460px`, `justify-self:end`, `position:relative`. It contains four stacked absolutely-positioned children:
  1. `.orbit` — `inset:-14%`, `border-radius:50%`, `border:1px dashed rgba(159,179,200,.18)`. A faint outer circle.
  2. `.ring` — `inset:0`, a **conic gradient ring**: `conic-gradient(from 40deg, transparent 0 100deg, var(--orange) 100deg 360deg)`, punched into a ring shape with a radial-gradient mask: `radial-gradient(farthest-side, transparent calc(100% - 32%), #000 calc(100% - 32% + 1px))` (both `-webkit-mask` and `mask`). The `from 40deg` + 100° transparent arc puts the gap on the right side, matching the logo's open C.
  3. Two `.ring-cap` dots — `width:16%`, `aspect-ratio:1`, `border-radius:50%`, `background:var(--orange)`, `transform:translate(-50%,-50%)`. Positioned inline via `style="left:77%;top:17.8%"` and `style="left:77%;top:82.2%"` — these are the rounded stroke ends that terminate the open C. **Their positions are tuned to sit exactly on the arc ends**; if you change the ring's mask thickness or the conic `from` angle, these must be re-derived or the ends will visually detach.
  4. `.ring-dot` — the green centre dot. `left:50%; top:50%`, `width:19%`, `aspect-ratio:1`, `background:var(--green)`, centered via translate. It has a pulsing halo `::after`: `inset:-60%`, `border:1.5px solid rgba(26,158,106,.5)`, `animation: pulse 3.6s ease-out infinite`, where `@keyframes pulse` scales from `.45` to `1.5` while fading opacity 1 → 0.
- **Below the grid — `.hero__facts`:** a 3-up fact strip (`grid-template-columns:repeat(auto-fit,minmax(210px,1fr))`), `margin-top:clamp(70px,9vw,110px)`, `border-top:1px solid rgba(159,179,200,.18)`. Each `.fact`:
  - `padding:30px 28px 38px 0`; adjacent facts get `padding-left:28px; border-left:1px solid rgba(159,179,200,.18)`
  - `<b>` — the big numeral: Space Grotesk, `clamp(38px,4.4vw,56px)`, `font-weight:500`, `line-height:1`, `letter-spacing:-.03em`, `color:var(--paper)`
  - `<strong>` — 16px, `var(--paper)`, `margin:14px 0 6px`
  - `<p>` — 14px, `var(--mist)`, `line-height:1.6`
  - Content, in order:
    | numeral | title | description |
    |---|---|---|
    | 2026 | 공식 출범 | 5월 15일 총회를 거쳐 출범, 초대 회장 추대 |
    | 2000 | 이어받은 뿌리 | 과학문화 NGO협의회 → 한국과학문화교육단체연합(2010, 50여 개 단체)의 노하우와 네트워크 계승 |
    | 2031 | 세계 무대 목표 | PCST Network 세계대회 한국 유치 로드맵 추진 |

### 2 — 학회 소개 (`#about`, `.sec.sec--white`)

- **Background:** `#ffffff` (`.sec--white`), text `var(--text)`.
- **Section padding:** `var(--sec)` = `clamp(80px,11vw,140px)` top and bottom.
- **Section head `.sec__head`:** two-column grid `280px 1fr`, `gap:40px`, `margin-bottom:clamp(44px,6vw,72px)`.
  - Left `.sec__num` — `01` (in `var(--orange)`) then `About KCST`. Space Grotesk, 13px, `letter-spacing:.16em`, uppercase, `color:var(--slate)`, `padding-top:12px`.
  - Right `<h2>` — `과학이 사회와 대화하는 방법을<br>연구하고 실천합니다`. `font-size:clamp(30px,4vw,50px)`.
- **`.ab-lead` — the identity statement.** This is the most prominent text in the section and its line breaks are a hard requirement.
  - `font-size: clamp(22px,2.5vw,36px)`, `font-weight:600`, `line-height:1.62`, `letter-spacing:-.025em`, `max-width:32em`.
  - `.ab-intro` is `display:block` (deliberately **not** a multi-column grid) so the three `<br>`-separated lines always render as exactly three lines. Earlier a 1.x/1fr grid caused line 2 to wrap into a 4th line — do not reintroduce a constrained column here.
  - Line 1 leads with `<strong>한국과학기술문화소통학회(KCST)</strong>` colored `var(--navy)`; the phrase `과학소통 전문 학회` is in `<mark>` — `background:none; color: var(--orange)`.
- **`.ab-body`** — supporting paragraphs. Two-column grid `1fr 1fr`, `gap:clamp(24px,3.4vw,56px)`, separated from the lead by `border-top:1px solid #DCE0DA`, `margin-top:clamp(30px,3.6vw,48px)`, `padding-top:clamp(26px,3vw,40px)`. Paragraphs: `font-size:17.5px`, `color:var(--text-2)` `#4A5A6B`, `line-height:1.9`. Paragraph 2 contains an intentional `<br>` after `시작해`.
- **`.ab-values` — three value cards.** `grid-template-columns:repeat(auto-fit,minmax(260px,1fr))`, `gap:20px`, `margin-bottom:20px`. Each `.val`: `background:var(--paper)` `#F4F5F2`, `border-radius:20px`, `padding:clamp(26px,3vw,34px)`.
  Each card has a CSS-drawn 56×56 mark, an English kicker, an `<h3>`, and a paragraph:
  | mark (`.val__mark--*`) | kicker | h3 | body |
  |---|---|---|---|
  | `--c` — conic gradient ring, masked, same 100° gap geometry as the hero ring but thinner | Openness | 열려 있는 과학 | 로고의 열린 C처럼, 과학의 문은 누구에게나 열려 있어야 합니다. 전공과 직업에 관계없이 누구나 대화에 참여할 수 있습니다. |
  | `--dot` — green filled circle (`inset:16px`) inside a translucent green ring (`inset:2px`) | People | 사람이 중심인 소통 | 대화의 출발점은 시민입니다. 일방적인 전달보다 듣고 함께 생각하는 소통으로 과학과 사람이 함께 자라는 관계를 만듭니다. |
  | `--bar` — two navy vertical bars (12px wide) at `left:6px` and `left:26px`, the second at `opacity:.35` | Depth | 근거가 단단한 소통 | 재미있되 정확해야 합니다. 학술 연구와 전문가 검증을 바탕으로, 신뢰할 수 있는 과학소통의 기준을 세웁니다. |
  - Kicker `.val__k`: Space Grotesk, 13px, `letter-spacing:.14em`, uppercase, `color:var(--slate)`.
  - `<h3>`: `clamp(21px,2vw,25px)`, `margin:8px 0 12px`.
  - Last `<p>`: 16px, `var(--text-2)`.
- **`.ab-who` — "함께하는 사람들".** Boxed panel: `grid-template-columns:280px 1fr`, `gap:40px`, `padding:40px`, `background:var(--paper)`, `border-radius:20px`, `align-items:center`. `<h3>` 18px. The audience list is a `flex-wrap` row of white pill tags (`border-radius:999px`, `padding:10px 18px`, 15px, `font-weight:500`, `background:#fff`) — five items, exact text:
  1. 대학·연구기관의 과학기술인
  2. 과학커뮤니케이터·작가·크리에이터
  3. 과학관·공공 과학문화 기관
  4. 과학카페·과학책방 등 민간 기업
  5. 교사·학생, 그리고 과학을 좋아하는 시민
- **`.ab-pres` — president block.** Full-width navy band: `display:flex`, `gap:26px`, `padding:32px 40px`, `border-radius:20px`, `background:var(--navy)`, `color:var(--paper)`, `margin-top:20px`.
  - `.ab-pres__mark` — 76×76 circle, `border-radius:50%`, `background:rgba(244,245,242,.08)`, `border:1px solid rgba(159,179,200,.18)`, `flex:0 0 auto`, centered image.
  - Inside: `<img src="kcst-symbol.png" width="275" height="275">` at `width:54%; height:auto`. **`height:auto` is required** — setting only a width while leaving the HTML `height` attribute in force stretches the symbol vertically, which was a reported bug.
  - Text: `.ab-pres__role` `초대 회장` (13px, `var(--green)`, 600) / `.ab-pres__name` `조숙경` + `<span>한국에너지공과대학교(KENTECH) 교수</span>` (22px / 15px, span in `#B8C4D0`) / `.ab-pres__desc` `세계과학커뮤니케이션학회(PCST Network) 아시아인 최초 회장(2023~2025)` (15px, `#B8C4D0`).

### 3 — 핵심 사업 (`#programs`, `.sec.sec--navy`)

- **Background:** `var(--navy)`; text `var(--paper)`. Section head uses `.sec--navy` overrides: `.sec__num` → `var(--mist)`, `.sec__intro` → `#B8C4D0`.
- **Section head:** `02` / `Programs`, then `<h2>KCST가 하는 일</h2>` and intro `학술 연구에서 현장 교육, 시민 참여와 국제 교류까지. 과학소통의 처음과 끝을 함께합니다.`
- **`.prog` grid:** `repeat(auto-fit,minmax(300px,1fr))`, `gap:20px`. Six cards.
- **Standard card `.pg`:** `background: var(--navy-2)` `#1B2B3D`, `border:1px solid rgba(159,179,200,.18)`, `border-radius:20px`, `padding:clamp(26px,3vw,36px)`, `display:flex; flex-direction:column`, `min-height:220px` (so rows align).
  - Hover: `border-color: rgba(228,87,46,.55)`, `transform: translateY(-2px)`; `transition: border-color .25s, transform .25s`.
  - `.pg__k` — Space Grotesk, 12px, `letter-spacing:.14em`, uppercase, `var(--mist)`, `margin-bottom:36px`. Format: `NN · English Label`.
  - `<h3>` `clamp(20px,1.9vw,24px)`, `margin-bottom:12px`.
  - `<p>` 16px, `#B8C4D0`.
- **Feature card `.pg.pg--feature` (the first one):** `grid-column: span 2` — so on a 3-column desktop grid it spans two cells and the remaining four cards fill the rest. `background:var(--orange)`, `border-color:var(--orange)`, `<h3>` `clamp(26px,3vw,38px)`, paragraph `rgba(255,255,255,.92)`. Its inner `<b>열린 세션</b>` is white + underlined (`text-underline-offset:3px`). At ≤700px `grid-column` resets to `auto` and `min-height` to `0`.
- **Tag row `.pg__tags`** (feature card only): flex-wrap pills, `margin-top:auto; padding-top:28px`, each `font-size:13px; padding:6px 14px; border-radius:999px; border:1px solid rgba(255,255,255,.45); color:#fff`. Six tags: 국제·학술 / 커뮤니케이터 / 연구기관 / 민간 생태계 / 과학관 / 열린 세션.
- **Card content (exact copy):**

  | kicker | h3 | body |
  |---|---|---|
  | 01 · Annual Conference (feature) | 정기 학술대회 | 매년 가을, 과학소통의 여러 현장이 한자리에 모입니다. 국제 학술, 연구기관, 과학관, 민간 과학문화 공간 등 분야별 세션에서 경험과 연구를 나누고, 시민 누구나 참여하는 **열린 세션**도 운영합니다. |
  | 02 · Fact × Impact | 과학자와 커뮤니케이터의 협업 | 전공 과학자의 깊이와 커뮤니케이터의 이야기 힘을 잇습니다. 강연과 콘텐츠를 함께 검토하고 다듬어 정확하면서도 잘 전달되는 과학을 만듭니다. |
  | 03 · Education & Certification | 교육·인증 | 과학소통 전문인력을 키우는 교육 과정과 인증 제도를 운영합니다. 고경력 과학기술인의 노하우가 다음 세대로 이어지도록 돕습니다. |
  | 04 · Public Dialogue | 과학기술 공론장 | AI, 에너지, 기후, 백신, 식품처럼 삶에 닿는 과학기술 이슈를 시민·과학계·정책 현장이 함께 숙의하는 자리를 엽니다. |
  | 05 · Research & Publishing | 연구·학술지·정책 자문 | 과학소통 분야의 연구를 축적하고 전문 학술지 창간을 준비합니다. 정부·지자체·공공기관의 과학문화 정책 연구와 자문에도 참여합니다. |
  | 06 · Global Network | 국제 교류 | 세계과학커뮤니케이션학회(PCST Network)의 아시아 거점으로서 한국의 과학소통을 세계에 알리고, 아시아 각국과 교류합니다. |

### 4 — 로드맵 (`#roadmap`, `.sec` on the paper background)

- **Background:** `var(--paper)` `#F4F5F2` (default `.sec`).
- **Section head:** `03` / `Roadmap`, `<h2>2031, 세계 과학소통의 무대를<br>한국으로</h2>`, intro `시민단체의 연대에서 시작한 KCST는 2031년 PCST Network 세계대회 한국 유치를 목표로 한 걸음씩 나아갑니다.`
- **`.phases`:** an `<ol>` with `list-style:none`, `grid-template-columns:repeat(auto-fit,minmax(230px,1fr))`, `gap:20px` — four phase cards in one row on desktop.
- **Base card `.ph`:** white, `border:1px solid #DCE0DA`, `border-radius:20px`, `padding:30px 26px`.
  - `.ph__step` — 12px, `letter-spacing:.14em`, uppercase, `var(--slate)`
  - `.ph__yr` — Space Grotesk, `clamp(28px,2.8vw,36px)`, `font-weight:500`, `letter-spacing:-.03em`, `margin:10px 0 18px`, `line-height:1`
  - `<h3>` 20px, `margin-bottom:8px`
  - `<p>` (direct child) 15px, `var(--text-2)`
  - `<ul>` milestone list: `margin-top:18px`, `border-top:1px solid #DCE0DA`; each `<li>` 14px, `padding:9px 0`, `border-bottom:1px solid #DCE0DA`
- **Phase 2 variant `.ph--now` (current phase):** navy card. `background:var(--navy)`, `border-color:var(--navy)`, text `var(--paper)`. `.ph__step` becomes `var(--green)` and gains an 8px green dot via `::before` with `box-shadow: 0 0 0 4px rgba(26,158,106,.25)`. List borders → `rgba(159,179,200,.18)`; body text → `#B8C4D0`; `<li><b>` → `var(--paper)`.
  It also contains `.ph__event`, an inner event card: `margin-top:20px; padding:18px; border-radius:14px; background:var(--navy-2); border:1px solid rgba(159,179,200,.18)`. Lines: label `창립대회` (12px, `var(--mist)`), then `11.17 PCST Asia 국제 토론회` and `11.18 창립기념 심포지엄 · 총회` with the dates in `var(--orange)`. Ends with `<p class="countdown" id="countdown">` — `font-size:22px`, `color:var(--paper)`, filled by JS.
- **Phase 4 variant `.ph--goal`:** `border:2px solid var(--orange)`; `.ph__step` and `.ph__yr` in `var(--orange)`; `.ph__yr` enlarged to `clamp(40px,4.4vw,60px)`.
- **Phase content (exact copy):**

  | step | year | h3 | body | milestones |
  |---|---|---|---|---|
  | Phase 01 | 2000–2025 | 뿌리를 내리다 | 과학문화 시민단체들이 연대해 현장의 경험과 네트워크를 쌓아 왔습니다. | 과학문화NGO협의회 결성 · 한국과학문화교육단체연합 창립 · 과학소통포럼 개최 |
  | Phase 02 · Now | 2026 | 학회로 공식 출범하다 | 사단법인 학회로 새롭게 출발해 첫 공식 무대를 엽니다. | 5월 창립총회, 초대 회장 추대 · 8월 첫 라운드톡 개최 · **11월 17–18일 창립대회** |
  | Phase 03 | 2027–2030 | 기반을 다지다 | 정기 학술대회와 학술지를 축으로 학회의 활동을 꾸준히 이어 갑니다. | 매년 정기 학술대회 개최 · 전문 학술지 창간 · 세계대회 개최 후보지 등록 |
  | Phase 04 · Goal | 2031 | 세계로 나아가다 | PCST Network 세계대회를 한국에 유치해 아시아 과학소통의 중심으로 자리매김합니다. | — |

### 5 — FAQ (`#faq`, `.sec.sec--white`)

- **Section head:** `04` / `FAQ`, `<h2>자주 묻는 질문</h2>`, intro `더 궁금한 점은 [leesh45337@gmail.com]으로 문의해 주세요.` — the address is a link styled `var(--orange)`, `font-weight:600`. **Note how the address is encoded — see "Email links are deliberately encoded" below.**
- **`.faq`:** `display:grid; gap:12px`. Eight `<details>` elements — each a **card**, not a divider list.
  - `.faq details`: `background:#fff`, `border:1px solid #DCE0DA`, `border-radius:16px`, `overflow:hidden`, `transition: border-color .25s, box-shadow .25s`.
  - Open state `details[open]`: `border-color: rgba(228,87,46,.45)`, `box-shadow: 0 12px 28px rgba(20,32,46,.06)`.
  - `<summary>`: `list-style:none` (and `::-webkit-details-marker{display:none}`), `cursor:pointer`, `padding:22px 60px 22px 26px`, `font-size:clamp(17px,1.6vw,20px)`, `font-weight:600`.
  - Chevron: `summary::after` is a 13×13 box with only right+bottom borders (2px, `var(--orange)`) rotated 45°, at `right:26px`, vertically centered. When open it rotates to `-135deg`.
  - Body `<p>`: `padding:0 60px 24px 26px`, 16px, `var(--text-2)`, with a `::before` hairline separating it from the summary.
- **First item is `open` by default.**
- **Fourth item has `id="faq-join"`** — the nav button and hero secondary CTA target it.
- **Questions and answers (exact copy) — these are duplicated into the FAQPage structured data, so keep the two in sync:**
  1. **한국과학기술문화소통학회(KCST)는 어떤 곳인가요?** — 과학기술과 사회의 소통을 연구하고 실천하는 사단법인 학회입니다. 학술대회·학술지·교육·인증·공론장·국제 교류 활동으로 과학소통의 전문성을 높이고, 국민 누구나 과학을 가깝게 이해할 수 있는 환경을 만듭니다. 2026년 5월 출범했으며 초대 회장은 조숙경 교수(KENTECH)입니다.
  2. **과학소통(과학커뮤니케이션)이란 무엇인가요?** — 과학기술의 지식과 쟁점을 시민과 나누고, 시민의 질문과 의견을 다시 과학계로 전하는 양방향의 활동을 말합니다. 강연·책·영상·전시는 물론, 과학기술 정책에 대한 시민 토론까지 모두 과학소통에 포함됩니다.
  3. **과학자나 연구자가 아니어도 가입할 수 있나요?** — 네. 과학커뮤니케이터, 작가와 크리에이터, 교사, 과학관과 공공기관 종사자, 과학문화 공간 운영자 등 과학소통에 관심 있는 분이라면 누구나 함께할 수 있습니다. 관련 분야 대학(원)생은 학생회원으로 가입할 수 있습니다.
  4. **회원 가입은 어떻게 하나요?** *(id `faq-join`)* — 입회원서를 제출하면 운영위원회 승인을 거쳐 회원이 됩니다. 회원은 정회원, 학생회원, 단체회원으로 나뉘며 정회원과 단체회원에게 총회 의결권과 선거권이 있습니다. 입회원서는 이메일로 요청해 주세요.
  5. **기관이나 기업도 함께할 수 있나요?** — 네. 단체회원으로 가입해 학회 활동에 참여할 수 있으며, 학술대회 공동 개최와 후원, 과학소통 관련 연구·교육 위탁도 함께 논의할 수 있습니다.
  6. **창립대회는 언제 열리나요? 누구나 참석할 수 있나요?** — 2026년 11월 17일(화)부터 18일(수)까지 열립니다. 첫날은 아시아 각국 대표가 참여하는 PCST Asia 국제 토론회, 둘째 날은 창립기념 심포지엄과 총회가 진행됩니다. 장소와 참가 신청 방법은 확정되는 대로 이 페이지에서 안내합니다.
  7. **일반 시민도 학술대회에 참여할 수 있나요?** — 네. 2027년부터 매년 열리는 정기 학술대회에는 회원이 아닌 시민도 무료 또는 최소 비용으로 참여할 수 있는 열린 세션이 마련됩니다.
  8. **PCST Network는 무엇이고, KCST와 어떤 관계인가요?** — PCST Network(Public Communication of Science and Technology)는 전 세계 과학소통 연구자와 실천가가 모인 국제 네트워크입니다. KCST는 그 아시아 거점 역할을 맡고 있으며, 2031년 PCST 세계대회를 한국에 유치하기 위한 준비를 이어 가고 있습니다.

### 6 — Footer (`.ft`)

- `background: var(--ink)` `#0F1A2B`, `color: var(--mist)`, `padding:60px 0 40px`, `font-size:14px`.
- `.ft__top`: flex row, `justify-content:space-between`, `padding-bottom:32px`, `border-bottom:1px solid rgba(159,179,200,.18)`. Contains the light horizontal logo at `height:40px; width:auto`, and a nav with the same four links as the header (학회 소개 / 핵심 사업 / 로드맵 / FAQ), `gap:22px`; link hover → `var(--paper)`.
- `.ft__bot`: flex row, `padding-top:24px`. Left: `<address>` (`font-style:normal`) `사단법인 한국과학기술문화소통학회 (KCST) · 문의 leesh45337@gmail.com` with the address as a link. Right: `© 2026 KCST. All rights reserved.` in Space Grotesk. The email link uses the same encoding scheme as the FAQ one.

---

## Interactions & Behavior

There are four pieces of runtime behavior. Everything else is static HTML/CSS.

### 1. Header state on scroll

```
onScroll: header.classList.toggle('is-solid', window.scrollY > 40)
```
Bound to `scroll` with `{passive:true}`, and invoked once on load (so a page opened mid-scroll is correct). No throttling is needed.

### 2. Mobile menu

Toggling adds/removes `is-open` on the `<header>` (not on the nav). This simultaneously reveals the dropdown panel and turns the hamburger into an X. `aria-expanded` on the button is updated to match, and `aria-label` flips between "메뉴 열기" and "메뉴 닫기". Any click on a nav link removes the class and resets `aria-expanded` to false. The panel is kept in the DOM at all widths (hidden by `opacity`/`pointer-events`), so focus handling should be verified in the target implementation — ideally the links should not be tabbable while closed.

### 3. Scroll-reveal animation

Elements marked `.rv` start at `opacity:0; transform:translateY(24px)` and transition to visible over `.8s ease` when they enter the viewport.

```
IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target); } });
}, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' })
```

- Each observed element gets a staggered `transitionDelay` of `(index % 3) * 80ms`.
- Elements are unobserved after revealing, so each animates once.
- **Fallback:** if `IntersectionObserver` is unavailable, every `.rv` is given `.in` immediately (fully visible). Any reimplementation must keep this fallback and must not leave content invisible when JS fails.
- `@media (prefers-reduced-motion: reduce)` disables the transform/transition on `.rv`, sets `.rv{opacity:1}`, stops the hero dot pulse, and switches `html{scroll-behavior:auto}`.

### 4. FAQ deep-link to the join item

Two mechanisms, both needed:

- Clicking any `a[href="#faq-join"]` sets `open = true` on `#faq-join` (in addition to the normal anchor scroll) — because navigating to a closed `<details>` scrolls to a collapsed element.
- On load and on every `hashchange`, if `location.hash` resolves to a `<details>`, that element is opened. This covers someone arriving from an external link or a search result pointing at `#faq-join`.

In a SPA/component framework, model the eight FAQ items as an array with an `open` flag so this stays declarative.

### 5. Founding-conference countdown

Fills `#countdown` inside the Phase 02 card in the roadmap:

- `start = 2026-11-17T00:00:00+09:00`, `end = 2026-11-19T00:00:00+09:00` (KST — the `+09:00` offset matters).
- `now < start` → `D-{Math.ceil((start - now) / 86400000)}` (renders as e.g. `D-26`)
- `start <= now < end` → `NOW ON · 창립대회 진행 중`
- `now >= end` → `창립대회를 성황리에 마쳤습니다`
- Computed once on load; a live countdown ticker is not required. Note the text is written directly into the page, so the element must exist before this runs.

### 6. Email links are deliberately encoded

The two contact-email links (FAQ intro, footer `<address>`) do **not** carry a plain `mailto:` in the HTML source. They are written as:

```html
<a class="mail" data-u="leesh45337" data-d="gmail.com">leesh45337&#64;gmail&#46;com</a>
```

and the script assembles the href at runtime:

```js
document.querySelectorAll('a.mail[data-u][data-d]').forEach(a => {
  a.href = 'mailto:' + a.dataset.u + '@' + a.dataset.d;
});
```

**Why:** the build/serve environment this design passed through rewrites literal email addresses in HTML into Cloudflare email-protection markup (`<a href="/cdn-cgi/l/email-protection#..."><span class="__cf_email__" data-cfemail="...">`), which leaves the source file with broken links that point at `/cdn-cgi/` paths that won't exist on the target host. Splitting the address into `data-` attributes avoids the rewriter entirely; the `&#64;` / `&#46;` entities in the visible text keep it readable while dodging a naive address pattern.

**What to do in the target codebase:** if that rewriter is not present (most React/Vue/Next.js or static-host pipelines), just write a normal `href="mailto:leesh45337@gmail.com"` — it is simpler and more robust. If you do keep the runtime assembly, keep the visible text as the plain address so it works without JS. Either way, verify the link actually opens a mail composer after deploy, and make sure no `/cdn-cgi/` or `__cf_email__` markup survives in the output.

### Anchor navigation

All in-page links use `id` targets: `#top`, `#about`, `#programs`, `#roadmap`, `#faq`, `#faq-join`. Smooth scrolling comes from `html{scroll-behavior:smooth; scroll-padding-top:72px}` — the scroll padding offsets the fixed 68px header (plus a little breathing room). **Do not use `scrollIntoView`** in the implementation; it fights the sticky header and can disturb the host page.

### Accessibility notes

- A skip link (`.skip`, visible on `:focus`) precedes the header and targets `#main`.
- `<main id="main">` wraps all six sections; `<footer>` and `<header>` are outside it.
- Each section is `aria-labelledby` pointing at its `<h2>` id. The hero uses `aria-labelledby="hero-title"`.
- The hero art is `aria-hidden="true"` (purely decorative); its inner logo images carry `alt=""`.
- The decorative `.val__mark` divs are `aria-hidden="true"`.
- The membership list is a real `<ul>`; the roadmap phases and program cards are `<ol>`/`<article>`.

---

## State Management

Minimal. No data fetching, no server state, no forms.

| State | Type | Where | Notes |
|---|---|---|---|
| `headerSolid` | boolean | computed from scroll position | Derive from scroll; don't store separately. |
| `menuOpen` | boolean | header | Drive `is-open`, `aria-expanded`, `aria-label` from one value. |
| `revealed` | boolean per `.rv` element | local | One-way (false → true), then unobserved. |
| `faqOpen` | boolean per FAQ item (8) | `#faq` | Item 4 (`faq-join`) also forced open by the deep-link handler. |
| `countdownText` | string | roadmap Phase 02 | Computed once at mount from the current time. |

No persistence is required in the design reference. If the target app has a pattern for remembering scroll position or open accordion state, applying it here would be reasonable but is not required.

---

## Design Tokens

All tokens are CSS custom properties on `:root` of `kcst.css`. **Colors and typefaces come from the official BI guidelines and must not be substituted.**

### Colors

| Token | Hex | Role |
|---|---|---|
| `--navy` | `#14202E` | **Primary.** KCST 네이비 (from BI). Hero, dark sections, Phase 02 card, president band. Also `--text`. |
| `--navy-2` | `#1B2B3D` | Raised surface on navy (program cards, inner event card). |
| `--ink` | `#0F1A2B` | Footer background — slightly darker than navy. |
| `--orange` | `#E4572E` | **Accent 1.** KCST 시그널 오렌지 (from BI). Section numbers, feature card, links, chevrons, Phase 04 emphasis, orange C. |
| `--green` | `#1A9E6A` | **Accent 2.** KCST 커뮤니티 그린 (from BI). Eyebrow dot, hero centre dot, Phase 02 marker, president role. |
| `--paper` | `#F4F5F2` | **Base surface.** KCST 페이퍼 (from BI). Page background, value cards, footer-adjacent panels. |
| `--slate` | `#5B7C99` | Muted kicker text, English subline, `.sec__num`. From the deck palette. |
| `--mist` | `#9FB3C8` | Lighter muted text on navy. |
| `--line` | `#DCE0DA` | Hairline borders/dividers on light backgrounds. |
| `--line-d` | `rgba(159,179,200,.18)` | Hairline borders/dividers on dark backgrounds. |
| `--text` | `#14202E` | Body text on light. Same value as `--navy`. |
| `--text-2` | `#4A5A6B` | Secondary body text on light. |
| — | `#C9D3DD` | Hero lead paragraph, card list text on dark. (Hard-coded.) |
| — | `#B8C4D0` | Body text on navy surfaces (`.sec--navy .sec__intro`, `.pg p`, `.ph--now` body). (Hard-coded.) |
| — | `rgba(244,245,242,.94)` | Solid header background (paper at 94%). |
| — | `rgba(228,87,46,.45)` | Open FAQ card border. |
| — | `rgba(255,255,255,.45)` | Feature-card tag pill border. |
| — | `#cf4a23` | Primary button hover (orange, darkened). |

**Usage rule from the BI:** navy and paper are the ground; orange and green are used sparingly as accents. Keep it that way.

### Typography

Two families, both free/open fonts, both loaded from Google Fonts. **Do not substitute** — these are the society's designated typefaces.

| Token | Stack | Role |
|---|---|---|
| `--f-en` | `'Space Grotesk', 'IBM Plex Sans KR', sans-serif` | English, numerals, event names. Weights 400/500/700. |
| `--f-kr` | `'IBM Plex Sans KR', 'Apple SD Gothic Neo', sans-serif` | Korean headings, body, documents. Weights 400/500/600/700. |

Load:
```
https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+KR:wght@400;500;600;700&family=Space+Grotesk:wght@400;500;700&display=swap
```
with `<link rel="preconnect">` to both `fonts.googleapis.com` and `fonts.gstatic.com` (the latter needs `crossorigin`).

Body: `font-family: var(--f-kr); line-height:1.7; -webkit-font-smoothing:antialiased`.

**Korean typesetting rules baked in — preserve these:**
- `word-break: keep-all` on `body` — without it Korean wraps mid-word and looks broken.
- `overflow-wrap: break-word` on `body` as a safety net.
- `text-wrap: balance` on `h1,h2,h3`; `text-wrap: pretty` on `p`.

Scale (all `clamp()`-based for fluid sizing):

| Element | Size | Weight | Line-height | Letter-spacing |
|---|---|---|---|---|
| Hero h1 | `clamp(40px,6.4vw,84px)` | 700 | 1.12 | `-.035em` |
| Section h2 | `clamp(30px,4vw,50px)` | 700 | 1.3 | `-.02em` |
| `.ab-lead` | `clamp(22px,2.5vw,36px)` | 600 | 1.62 | `-.025em` |
| `.hero__lead` | `clamp(17px,1.6vw,20px)` | 400 | 1.7 | — |
| `.sec__intro` | `clamp(16px,1.4vw,18px)` | 400 | 1.7 | — |
| `.ph__yr` | `clamp(28px,2.8vw,36px)` | 500 | 1 | `-.03em` |
| `.ph--goal .ph__yr` | `clamp(40px,4.4vw,60px)` | 500 | 1 | `-.03em` |
| `.fact b` | `clamp(38px,4.4vw,56px)` | 500 | 1 | `-.03em` |
| `.pg--feature h3` | `clamp(26px,3vw,38px)` | 700 | 1.3 | `-.02em` |
| `.pg h3` | `clamp(20px,1.9vw,24px)` | 700 | 1.3 | `-.02em` |
| `.val h3` | `clamp(21px,2vw,25px)` | 700 | 1.3 | `-.02em` |
| `.faq summary` | `clamp(17px,1.6vw,20px)` | 600 | 1.7 | — |
| Body copy | 16–17.5px | 400 | 1.7–1.9 | — |
| Kickers (`.eyebrow`, `.sec__num`, `.val__k`, `.pg__k`, `.ph__step`) | 12–13px | 500 | — | `.14em`–`.16em`, uppercase |
| Footer | 14px | 400 | 1.7 | — |

Note the design is deliberately generous with type: nothing on the page is below 12px, and body copy sits at 16–17.5px. On a 1920-wide screen the hero h1 reaches 84px. Do not shrink these.

### Spacing

| Token / value | Use |
|---|---|
| `--wrap: 1200px` | `.wrap` max-width — the content column. |
| `--gut: clamp(20px,5vw,48px)` | `.wrap` horizontal padding (gutters). |
| `--sec: clamp(80px,11vw,140px)` | Vertical padding of every `.sec`. |
| `.sec__head` margin-bottom | `clamp(44px,6vw,72px)` |
| Grid gap | `20px` for all card grids; `40px` inside `.sec__head` and `.ab-who` |
| Card padding | `24–40px`, mostly `clamp(26px,3vw,36px)` |
| Header height | `68px` (fixed; matched by `scroll-padding-top:72px`) |

Also in use: `clamp(70px,9vw,110px)` (hero facts top margin), `clamp(30px,3.6vw,48px)` / `clamp(26px,3vw,40px)` (`.ab-body` separation), `clamp(130px,18vw,190px)` (hero top padding).

### Radii

| Value | Applied to |
|---|---|
| `999px` | Buttons, pill tags, eyebrow-adjacent dots |
| `50%` | Hero art circles, `.ab-pres__mark`, `.val__mark--c`, `.val__mark--dot` |
| `20px` | All cards: `.pg`, `.ph`, `.val`, `.ab-who`, `.ab-pres` |
| `16px` | `.faq details` |
| `14px` | `.ph__event` |
| `2px` | `.val__mark--bar` bars |
| `6px` | (no longer used — retained only if a bar chart is reintroduced) |

### Shadows

| Value | Applied to |
|---|---|
| `0 1px 0 #DCE0DA` | Solid header bottom edge (a hairline, not a shadow) |
| `0 12px 28px rgba(20,32,46,.06)` | Open FAQ card |
| `0 12px 30px rgba(20,32,46,.12)` | Mobile nav dropdown panel |
| `0 1px 0 rgba(20,32,46,.05)` | Membership pill tags |

Note the aesthetic: shadows are used almost not at all. Depth comes from background-value contrast (navy vs paper vs white) and hairlines. Keep it flat.

### Motion

| Property | Value |
|---|---|
| Scroll reveal | `opacity .8s ease, transform .8s ease`; stagger `(i % 3) * 80ms` |
| Header background/shadow | `.3s` |
| Card hover (`.pg`) | `border-color .25s, transform .25s` |
| FAQ card | `border-color .25s, box-shadow .25s` |
| FAQ chevron | `transform .25s` |
| Mobile nav panel | `.25s` |
| Button | `transform .2s, background .2s` |
| Hero dot pulse | `pulse 3.6s ease-out infinite` |
| Smooth scroll | `scroll-behavior: smooth` (disabled under reduced-motion) |

---

## Responsive Behavior

Fluid first: sizes use `clamp()` and card grids use `auto-fit` + `minmax()`, so most layouts reflow without breakpoints. Breakpoints exist for structural changes.

### `auto-fit` grid floors (the primary mechanism)

| Grid | Floor | Desktop result |
|---|---|---|
| `.prog` | `minmax(300px,1fr)` | 3 columns; feature card spans 2 |
| `.phases` | `minmax(230px,1fr)` | 4 columns |
| `.ab-values` | `minmax(260px,1fr)` | 3 columns |
| `.hero__facts` | `minmax(210px,1fr)` | 3 columns |

### `@media (max-width: 1080px)`

- `.prog` → explicit 2 columns; `.phases` → explicit 2 columns
- `.sec__head` → `200px 1fr` (narrower label column)

### `@media (max-width: 860px)` — tablet/mobile

- **Header:** hamburger `display:block`; nav becomes the fixed dropdown panel (see Header).
- **Hero:** `.hero__grid` → `1fr`. `.hero__art` is taken out of flow — `position:absolute; right:-22%; top:90px; width:62vw; max-width:340px; opacity:.28; z-index:0` — becoming a faint background watermark behind the headline. The first grid child gets `position:relative; z-index:1` to sit above it. `.hero` retains `overflow:hidden`, which clips the ring/orbit that overhang the viewport and prevents horizontal scroll. **This clipping is load-bearing — removing `overflow:hidden` from `.hero` will introduce horizontal scrolling on mobile.**
- **Hero facts:** `.hero__facts` → 1 column. Each `.fact` becomes a two-column grid `110px 1fr` with the numeral spanning both rows (`grid-row:1/3`), left-aligned at 38px; separators become bottom borders instead of left borders.
- `.sec__head` → `1fr`, `gap:12px`; `.sec__num` loses its `padding-top`.
- `.ab-intro` stays `display:block` (lead flows naturally); `.ab-body` → 1 column, `gap:18px`.
- `.ab-pres`: `align-items:flex-start; padding:28px`; the `<span>` with the affiliation drops to its own line (`display:block`).
- `.ab-who` → 1 column, `gap:20px`, `padding:28px`.

### `@media (max-width: 700px)`

- `.prog`, `.phases` → 1 column; `.pg--feature` resets `grid-column:auto`; `.pg` `min-height:0`; `.pg__k` margin tightened to 22px.

### `@media (max-width: 520px)` — small phones

- Header logo height 34px → **28px**.
- FAQ summary `padding:20px 54px 20px 20px`, `font-size:17px`; FAQ body `padding:0 24px 22px 20px`.
- Hero CTA buttons stretch to full width (`flex:1; justify-content:center`).

### `@media (prefers-reduced-motion: reduce)`

Reveal animations off (`.rv` fully visible, no transition), hero dot pulse stopped, smooth scrolling disabled.

**Touch targets:** the hamburger is 44×44px. Buttons are `padding:12px 22px` at 15px text (~46px tall). Do not reduce these.

---

## SEO & GEO Requirements

This is a functional specification, not decoration. The page must be crawlable and correctly summarized by Naver, Google, and AI answer engines.

### Required `<head>` markup

- `<html lang="ko">`
- `<title>`, `meta[name=description]`, `meta[name=keywords]`, `meta[name=author]`
- `<meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1">`
- `<link rel="canonical">` — **currently a placeholder domain, see Placeholders**
- `<meta name="theme-color" content="#14202E">`
- **Naver:** `<meta name="naver-site-verification" content="...">` — placeholder code present, must be replaced with the code issued by Naver 서치어드바이저
- **Google:** `<meta name="google-site-verification" content="...">` — same, from Google Search Console
- **Open Graph** (for Naver/KakaoTalk link previews): `og:type=website`, `og:site_name`, `og:locale=ko_KR`, `og:title`, `og:description`, `og:url`, `og:image` (1200×630), `og:image:width`, `og:image:height`, `og:image:alt`
- **Twitter card:** `summary_large_image` + title/description/image

### Structured data (JSON-LD in `<head>`)

A single `<script type="application/ld+json">` containing an `@graph` with five nodes. **Keep this pattern and keep it in the `<head>`.** Node `@id`s are interlinked (`#org`, `#president`, `#website`), so preserve the `@id` values when porting.

1. **`Organization` + `NGO`** (`#org`) — `name` 한국과학기술문화소통학회, `alternateName` [KCST, English full name, …], `legalName` 사단법인 … , `url`, `logo`, `image`, `description`, `foundingDate: 2026-05-15`, `slogan`, `areaServed: KR`, `knowsAbout` [과학소통, 과학커뮤니케이션, 과학문화, …], `founder` → `#president`, `member` → PCST Network, `parentOrganization` → 한국과학문화교육단체연합.
2. **`Person`** (`#president`) — `name` 조숙경, `jobTitle` 초대 회장, `affiliation` KENTECH, `description` with the PCST Network first-Asian-president fact.
3. **`WebSite`** (`#website`) — `inLanguage: ko-KR`, `publisher` → `#org`.
4. **`Event`** — the 2026 founding conference: `startDate 2026-11-17`, `endDate 2026-11-18`, `eventStatus: EventScheduled`, `eventAttendanceMode: OfflineEventAttendanceMode`, `location` (currently `장소 추후 공지` — needs the real venue), `organizer` → `#org`.
5. **`FAQPage`** with all **eight** questions/answers from the FAQ section, verbatim.

**Critical maintenance rule:** the FAQPage answers and the visible FAQ text are the same content in two places. If one changes, the other must change with it, or Google will flag the mismatch.

### Companion files (must be deployed at the site root)

- **`robots.txt`** — `Allow: /` for `*`, and explicit `Allow: /` blocks for `Yeti` (Naver) and the AI crawlers `GPTBot`, `OAI-SearchBot`, `ChatGPT-User`, `PerplexityBot`, `ClaudeBot`, `Google-Extended`. Ends with the absolute `Sitemap:` URL (placeholder domain).
- **`sitemap.xml`** — single `<url>` entry for the homepage, `lastmod 2026-10-21`, `changefreq weekly`, `priority 1.0`.
- **`llms.txt`** — a Markdown fact sheet for AI answer engines: one-line definition blockquote, then 핵심 사실 (name, founding date, president, lineage, international link, goal, slogan), 창립대회 dates, 주요 사업, 핵심 사업 방향 (4), 정기 학술대회 6대 세션, 회원 types, and links. Keep its facts consistent with the page.

### Content-level GEO patterns to preserve

- The `.ab-lead` sentence is a one-sentence definition of the society using its full official name — this is what answer engines quote. Keep it a single, self-contained, factual sentence.
- The FAQ section answers the "what is this / who can join / how do I join" cluster that generates AI-search traffic. Keep the questions in natural Korean question form.
- Facts are stated plainly and dated (2026-05-15 founding, 2026-11-17–18 conference, 2031 goal) rather than as marketing adjectives.

---

## Assets

All assets live in `assets/`. **Note:** the images below were traced back to the source BI documents but the exact 16-variant export bundle specified in the guidelines (PNG 600dpi, AI/PDF vector originals) was not available in this project; two files here are *derived* from the supplied logo PNG by cropping/regenerating. Flagged accordingly.

| File | Origin | Notes |
|---|---|---|
| `logo-h-dark.png` | Supplied BI asset | Horizontal lockup, light artwork for dark backgrounds, 1024×299, transparent. Used in the transparent header. |
| `logo-h-light.png` | Supplied BI asset | Horizontal lockup, dark artwork for light backgrounds, 1024×299, transparent. Used in the solid header. |
| `logo-v-dark.png` | Supplied BI asset | Vertical (signature) lockup, light artwork, 1024×597. Used as the source for the OG image; also available for reuse. |
| `logo-v-light.png` | Supplied BI asset | Vertical (signature) lockup, dark artwork, 1024×597. Source for the symbol extraction and the JSON-LD `logo` URL. |
| `kcst-symbol.png` | **Derived** by cropping the orange C + green dot out of `logo-v-light.png` and re-canvassing as a centred square (275×275), transparent. | Used in the president block and as the favicon. **Preferred fix: replace with the official 심볼 from the BI bundle** (`PNG 600dpi, 투명 배경`). |
| `favicon-512.png` | Derived (a 512×512 render of the symbol). | Favicon source. **Replace with the official 파비콘 / 앱 아이콘 (ICO · SVG · PNG 16–512px)** from the BI bundle. |
| `apple-touch-icon.png` | Derived (180×180, symbol on a `#F4F5F2` rounded square, 36px corner radius). | iOS home-screen icon. Same replacement advice. |
| `og-image.png` | **Generated** for this project — 1200×630 navy canvas with the vertical light logo centred, plus the English society name beneath in `#9FB3C8`. | Social/link-preview image referenced by `og:image` and `twitter:image`. Regenerate at higher quality if a designer is available. |

**Also supplied (source material, not web assets):** `KCST_소개자료.pptx` (the society's introduction deck — the source of most factual content), `KCST BI 가이드라인.pdf` (the brand guidelines, v1.0, 2026.10 — the authority for colors, typefaces, and logo rules). These are in `src/` in the design project. The BI PDF also documents logo clear space (≥ ½ the C height on all sides), minimum sizes (signature 200px wide on screen, wordmark 80px, symbol 24px), and prohibited uses (no stretching, rotating, recoloring, shadows, or placement on busy backgrounds) — those rules apply to how these assets are used in the implementation too.

---

## Placeholders To Replace Before Launch

These are known and intentional gaps. **Do not ship them as-is.**

1. **Domain** — `https://www.kcst.or.kr/` appears in `canonical`, `hreflang`, `og:url`, `og:image`, `twitter:image`, three `@id` values, the Organization/WebSite/Event `url` fields, and `sitemap.xml` + `robots.txt`. There is an HTML comment at the top of `index.html` marking this. Replace the whole domain in one pass with the real address once it is confirmed.
2. **Verification codes** — `naver-site-verification` (`NAVER_VERIFICATION_CODE`) and `google-site-verification` (`GOOGLE_VERIFICATION_CODE`). Both need the real values from Naver 서치어드바이저 and Google Search Console. Submit `sitemap.xml` in both consoles after launch.
3. **Event location** — the `Event` structured data has `"name":"장소 추후 공지"` and only `addressCountry: "KR"`. Fill in venue name and full postal address when confirmed. The on-page FAQ answer about the founding conference also says the venue will be announced on this page.
4. **Membership application link** — there is currently no application form. The page asks visitors to email for the 입회원서. If a Google Form, Naver Form, or application page is created, replace the contact-with-email instruction in FAQ item 4 and add a real CTA button.
5. **Contact email** — `leesh45337@gmail.com` is used in the FAQ intro and the footer address. Confirm this is the intended public contact, or swap in an official society address (and update both places).
6. **Founding conference details** — dates (2026.11.17–18) are fixed in the countdown and structured data, but registration method, fee, and program are not yet reflected. Add when available.
7. **Official logo bundle** — replace the derived `kcst-symbol.png`, `favicon-512.png`, and `apple-touch-icon.png` with the official exports from the BI bundle (see Assets).
8. **Real photography** — the design currently uses no photographs (deliberately: it relies on the logo geometry, color fields, and typography). If society photography is added later (president portrait, conference photos, science-café imagery), it must be placed by a designer, not auto-inserted, and should follow the BI rule about not placing the logo on busy backgrounds.

---

## Files

In this handoff bundle:

| File | What it is |
|---|---|
| `index.html` | The complete page: markup, `<head>` SEO/GEO metadata, JSON-LD `@graph`, and the one inline `<script>` with all runtime behavior. 391 lines, 19.0 KB. |
| `kcst.css` | All styling. Design tokens on `:root`, section-by-section rules, reveal animation, and all media queries. 229 lines, 15.6 KB. Linked as `kcst.css?v=4` — **keep a cache-busting query or a build hash on this link**, because during development a stale cached stylesheet repeatedly made fixed styles appear broken. |
| `robots.txt` | Crawler policy, including Naver Yeti and AI crawlers. Deploy at the site root. |
| `sitemap.xml` | Single-URL sitemap. Deploy at the site root. |
| `llms.txt` | AI-answer-engine fact sheet. Deploy at the site root. |

In the design project (not copied here, for reference if you have project access):

| File | What it is |
|---|---|
| `assets/` | All images listed in the Assets table above. |
| `src/KCST_소개자료.pptx` | Source introduction deck (factual source material). |
| `src/KCST_BI.pdf` | Brand identity guidelines v1.0 (the authority for colors, typefaces, logo usage). |

> An auto-generated `index.html.srcmap.json` sidecar may sit alongside `index.html`. It is an editor artifact, not part of the design — ignore or exclude it.

**Also present in the design project (a build-free deploy bundle, not part of this handoff):** `deploy/` contains the same HTML/CSS plus the four companion files, the `assets/` folder, and a Korean-language deployment checklist (`deploy/README.md`). Its README covers hosting options, the exact list of placeholder values to replace, and a post-deploy verification checklist. Useful to ship as-is; not needed to reimplement the design.

### Implementation checklist

- [ ] Choose the target environment (existing codebase, or Astro/Next.js static export if starting fresh)
- [ ] Port `:root` tokens as-is into whatever token system the project uses
- [ ] Load IBM Plex Sans KR (400/500/600/700) and Space Grotesk (400/500/700); keep the preconnects
- [ ] Keep `word-break: keep-all`, `text-wrap: balance`, and `text-wrap: pretty` — Korean typography depends on them
- [ ] Reproduce the header's two states from a single derived boolean
- [ ] Model the 8 FAQ items as data with an `open` flag; wire the `#faq-join` deep link
- [ ] Reimplement the reveal animation with a no-IntersectionObserver fallback that shows everything
- [ ] Copy the JSON-LD `@graph` verbatim, `@id`s included
- [ ] Deploy `robots.txt`, `sitemap.xml`, `llms.txt` at the site root
- [ ] Replace every item in "Placeholders To Replace Before Launch"
- [ ] Verify no horizontal overflow at 390px, and that the mobile menu is not tabbable while closed
- [ ] Confirm both email links open a mail composer, and that no `/cdn-cgi/` or `__cf_email__` markup survives into the deployed output
