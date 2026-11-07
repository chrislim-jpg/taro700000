# 오늘의 타로 — Design Guide

> 하얀 갤러리 위에 카드 한 장. 뽑은 카드 그림이 화면의 주인공이고, 글씨와 버튼은 그 주변에서 조용히 받쳐 준다.

**Theme:** light (다크 모드는 MVP 제외)
**기준 화면:** 모바일 375px 폭 (최대 콘텐츠 폭 440px, 데스크톱에서는 가운데 정렬)
**근거 문서:** TARO_01_PRD · TARO_02_기능명세서 · TARO_03_유저플로우 · TARO_04_와이어프레임
**구현 기준:** Tailwind CSS v4 (`@theme` CSS 설정) + shadcn/ui 방식 컴포넌트 (CVA 변형, 시맨틱 토큰)
**피그마:** https://www.figma.com/design/2vT8Ace7sfFq1vtRq5dhiC — 화면 9개 + 저장 이미지, 이 문서의 토큰을 변수 · 글씨 스타일 · 컴포넌트로 옮김

레퍼런스 사이트의 "제품이 주인공인 하얀 갤러리" 구성을 가져오되, 주인공을 **카드 한 장**으로 바꿨다. 하얀 바탕 · 연회색 밴드 · 28px 둥근 카드 · 그림자 없는 평면 · 작은 알약형 버튼 · 포인트 색 하나라는 절제는 그대로 두고, 포인트 색은 파랑 대신 **밤하늘 보라**, 보조 강조는 **달빛 금색**으로 바꿨다. 한글 화면이므로 글꼴은 Pretendard를 쓴다.

---

## 1. 디자인 원칙

| # | 원칙 | 화면에서 지키는 법 | 근거 |
|---|---|---|---|
| 1 | **카드가 주인공** | 결과 화면에서 카드 그림이 가장 크다. 카드 주변에는 흰 여백을 두고 상자형 UI를 겹치지 않는다. | PRD 해결 방안 · 와이어 S-03 |
| 2 | **짧고 조용하게** | 한 화면에 굵은 제목은 하나. 해석은 3줄 · 90자 안. 포인트 색은 화면당 주요 버튼 1개에만. | PRD 원칙 3 |
| 3 | **안심이 보이게** | 개인정보 안내(🔒)는 입력칸 바로 아래와 결과 버튼 아래에 항상 보이게. 접거나 숨기지 않는다. | PRD 원칙 1 |
| 4 | **오락이라고 분명히** | "※ 오락용 타로 해석입니다"를 결과 화면 · 저장 이미지 · 공유 글에 모두 넣는다. | PRD 원칙 4 |
| 5 | **1분 안에 끝** | 화면 3개, 큰 터치 영역(44px 이상), 빈칸이 있으면 버튼만 비활성. 팝업 · 모달로 흐름을 끊지 않는다. | PRD 제품 목표 |

---

## 2. Tokens — Colors

### 2-1. 브랜드 팔레트

| Name | Value | Token | Role |
|------|-------|-------|------|
| Gallery White | `#ffffff` | `--color-gallery-white` | 기본 화면 바탕, 결과 카드 바탕, 입력칸 바탕 |
| Moon Mist | `#f5f5f7` | `--color-moon-mist` | 주제 선택 밴드, 안내 박스, 결과 화면 버튼 영역 바탕 |
| Paper Frost | `#fafafc` | `--color-paper-frost` | 입력칸 포커스 전 옅은 채움, 대체 화면(길게 눌러 저장) 바탕 |
| Hairline | `#d6d6d6` | `--color-hairline` | 1px 구분선, 입력칸 기본 테두리 |
| Control Gray | `#e6e6e8` | `--color-control-gray` | 비활성 버튼 채움, 선택 안 된 주제 칩 테두리 |
| Ink | `#1d1d1f` | `--color-ink` | 제목, 본문, 해석 문장, 아이콘 |
| Slate | `#6e6e73` | `--color-slate` | 보조 문구, 개인정보 · 오락용 안내, 라벨 보조 설명 |
| Steel | `#86868b` | `--color-steel` | 바깥선 버튼 테두리, 비활성 글씨 (본문 글씨로는 쓰지 않음) |
| **Night Violet** | `#5b3fd1` | `--color-night-violet` | **주요 버튼 채움**([타로 시작] · [카드 한 장 뽑기]), 선택된 주제 칩, 포커스 링 |
| Violet Ink | `#4a32b0` | `--color-violet-ink` | 주요 버튼 눌림(active) · 호버, 글 링크 |
| Violet Haze | `#efebfc` | `--color-violet-haze` | 선택된 주제 칩 바탕, 카드 뒷면 바탕 |
| **Moon Gold** | `#b8902f` | `--color-moon-gold` | 카드 테두리 · 카드 번호(XIX) · 별자리 기호 장식. 넓은 면적에 쓰지 않음 |
| Gold Ink | `#8a6a1f` | `--color-gold-ink` | 흰 바탕 위 금색 글씨가 필요할 때(작은 라벨) — 대비 확보용 |
| Error Red | `#c4321f` | `--color-error-red` | 생년월일 오류 문구 · 입력칸 오류 테두리 |

**대비 확인 (WCAG AA 4.5:1 이상)**
- 흰 글씨 on Night Violet `#5b3fd1` ≈ 6.8:1 ✓
- Ink on Gallery White ≈ 16.8:1 ✓ / Slate on Gallery White ≈ 5.1:1 ✓ / Slate on Moon Mist ≈ 4.7:1 ✓
- Gold Ink on Gallery White ≈ 5.1:1 ✓ (Moon Gold `#b8902f`는 약 3:1이라 **장식 · 테두리 전용**, 글씨에 쓰지 않음)
- Error Red on Gallery White ≈ 5.5:1 ✓

### 2-2. 시맨틱 토큰 (shadcn 이름 체계)

컴포넌트는 브랜드 색 이름을 직접 쓰지 않고 아래 시맨틱 토큰만 쓴다. (`bg-primary` ○ / `bg-[#5b3fd1]` ✕)

| Semantic token | 값 | Tailwind 클래스 예 | 쓰는 곳 |
|---|---|---|---|
| `--color-background` | Gallery White | `bg-background` | 화면 바탕 |
| `--color-foreground` | Ink | `text-foreground` | 기본 글씨 |
| `--color-primary` | Night Violet | `bg-primary` | 주요 버튼, 선택 상태 |
| `--color-primary-foreground` | Gallery White | `text-primary-foreground` | 주요 버튼 글씨 |
| `--color-primary-hover` | Violet Ink | `hover:bg-primary-hover` | 주요 버튼 눌림 |
| `--color-secondary` | Moon Mist | `bg-secondary` | 보조 버튼 채움, 밴드 |
| `--color-secondary-foreground` | Ink | `text-secondary-foreground` | 보조 버튼 글씨 |
| `--color-muted` | Moon Mist | `bg-muted` | 안내 박스 바탕 |
| `--color-muted-foreground` | Slate | `text-muted-foreground` | 안내 · 보조 문구 |
| `--color-accent` | Violet Haze | `bg-accent` | 선택된 칩 바탕, 카드 뒷면 |
| `--color-accent-foreground` | Violet Ink | `text-accent-foreground` | 선택된 칩 글씨 |
| `--color-gold` | Moon Gold | `border-gold` | 카드 테두리 · 장식 |
| `--color-gold-foreground` | Gold Ink | `text-gold-foreground` | 작은 금색 라벨 |
| `--color-destructive` | Error Red | `text-destructive` | 오류 문구 |
| `--color-card` | Gallery White | `bg-card` | 결과 카드 |
| `--color-card-foreground` | Ink | `text-card-foreground` | 결과 카드 글씨 |
| `--color-border` | Hairline | `border-border` | 기본 테두리 |
| `--color-input` | Steel | `border-input` | 입력칸 · 바깥선 버튼 테두리 |
| `--color-ring` | Night Violet | `ring-ring` | 키보드 포커스 링 |
| `--color-disabled` | Control Gray | `bg-disabled` | 비활성 버튼 채움 |

### 2-3. 색 쓰는 규칙

- 보라(primary)는 **화면당 주요 버튼 1개 + 선택 상태**에만. S-03에서는 4개 버튼 중 아무것도 보라로 채우지 않는다 (카드가 주인공).
- 금색은 **카드 안에서만** 쓴다. 버튼 · 배경 · 큰 글씨에 금색 금지.
- 배경은 Gallery White와 Moon Mist 두 단계만. 그라디언트 금지 — 색과 깊이는 카드 그림이 맡는다.
- 주제 이모지(💼💰💕🌿📚✨)와 카드 그림 말고는 컬러 아이콘을 쓰지 않는다.

---

## 3. Tokens — Typography

### Pretendard — 모든 글씨 · `--font-sans`
- **대체 글꼴:** Pretendard Variable → Apple SD Gothic Neo → Noto Sans KR → system-ui → sans-serif
- **로드:** `cdn.jsdelivr.net/gh/orioncactus/pretendard` 동적 서브셋 (한글 용량 절약)
- **Weights:** 400, 500, 600, 700(카드 이름만)
- **한글 자간:** 레퍼런스의 강한 음수 자간은 한글에서 글자가 붙어 보이므로 약하게 쓴다 (본문 -0.2px 안팎, 큰 제목 -0.02em)
- **줄바꿈:** 한글 문장은 `word-break: keep-all`로 단어 단위 줄바꿈 (해석 문장이 어절 중간에서 끊기지 않게)
- **피그마 대체:** 피그마에 Pretendard가 없으면 Noto Sans KR을 쓴다. Noto Sans KR에는 600이 없으므로 400 → Regular, 500 → Medium, 600 · 700 → Bold로 맞춘다. Pretendard를 설치하면 `taro/*` 글씨 스타일의 글꼴만 바꾼다.

### Noto Serif KR — 카드 이름 전용 · `--font-card`
- 카드 그림 아래 "XIX 태양" 같은 카드 이름에만 쓴다. 타로다운 고전적 분위기를 이 한 곳에서만 낸다.
- **Weights:** 600 · **대체:** serif

### Type Scale

| Role | Family | Weight | Size | Line Height | Letter Spacing | Token | 쓰는 곳 |
|------|--------|--------|------|-------------|----------------|-------|---|
| caption | Pretendard | 400 | 12px | 1.33 | -0.1px | `--text-caption` | 오락용 안내, 이미지 하단 서비스 이름 |
| notice | Pretendard | 400 | 13px | 1.45 | -0.15px | `--text-notice` | 🔒 개인정보 안내, 입력칸 보조 설명, 오류 문구 |
| label | Pretendard | 600 | 14px | 1.3 | -0.2px | `--text-label` | 입력칸 라벨, 주제 칩, 작은 버튼 |
| body | Pretendard | 400 | 16px | 1.5 | -0.25px | `--text-body` | 시작 화면 소개, 일반 본문 |
| reading | Pretendard | 500 | 17px | 1.6 | -0.3px | `--text-reading` | **해석 3줄** (가장 잘 읽혀야 하는 글) |
| button | Pretendard | 600 | 17px | 1 | -0.2px | `--text-button` | 큰 버튼 글씨 |
| kicker | Pretendard | 600 | 15px | 1.2 | 0px | `--text-kicker` | "지민님의 타로 · 커리어" |
| card-name | Noto Serif KR | 600 | 22px | 1.2 | 0px | `--text-card-name` | "XIX 태양 · ♌ 사자자리" |
| title | Pretendard | 700 | 24px | 1.3 | -0.02em | `--text-title` | S-02 "나에 대해 알려 주세요" |
| hero | Pretendard | 700 | 34px | 1.2 | -0.02em | `--text-hero` | S-01 "오늘의 타로" |

- 한 화면에 `hero` 또는 `title`은 하나만.
- 해석 문장은 `reading`(17px/500)으로 화면에서 가장 읽기 편한 글씨. 16px 아래로 줄이지 않는다.
- 숫자(생년월일 · 카드 번호)는 `font-variant-numeric: tabular-nums`.

---

## 4. Tokens — Spacing & Shapes

**Base unit:** 4px · **Density:** comfortable (모바일 한 손 사용)

### Spacing Scale

| Name | Value | Token | 쓰는 곳 |
|------|-------|-------|---|
| 1 | 4px | `--spacing-1` | 아이콘과 글자 사이 |
| 2 | 8px | `--spacing-2` | 칩 사이, 라벨과 입력칸 사이 |
| 3 | 12px | `--spacing-3` | 버튼 2개 사이, 해석 줄 묶음 안 |
| 4 | 16px | `--spacing-4` | **화면 좌우 여백**, 입력 필드 묶음 사이 |
| 5 | 20px | `--spacing-5` | 관련 요소 묶음 사이 (element gap) |
| 6 | 24px | `--spacing-6` | 결과 카드 안쪽 여백 |
| 8 | 32px | `--spacing-8` | 화면 안 큰 덩어리 사이 |
| 12 | 48px | `--spacing-12` | 시작 화면 위아래 여백 |

Tailwind v4는 `--spacing: 4px` 하나로 `p-4 = 16px`처럼 계산되므로 위 표는 클래스 숫자와 같다.

### Layout

| 항목 | 값 |
|---|---|
| 화면 좌우 여백 | 16px (`px-4`) |
| 최대 콘텐츠 폭 | 440px (`max-w-[--container-app]`) |
| 요소 묶음 사이 | 20px |
| 큰 덩어리 사이 | 32px |
| 하단 고정 버튼 영역 | 안쪽 16px + `env(safe-area-inset-bottom)` |
| 최소 터치 영역 | 44×44px |

### Border Radius

| Element | Value | Token |
|---------|-------|-------|
| 결과 카드 · 안내 박스 · 대체 화면 이미지 | 28px | `--radius-card` |
| 타로 카드 그림 (실물 카드 모서리) | 14px | `--radius-tarot` |
| 입력칸 | 14px | `--radius-input` |
| 주제 칩 | 9999px | `--radius-pill` |
| 버튼 | 9999px | `--radius-pill` |
| 토스트 | 9999px | `--radius-pill` |

입력칸은 레퍼런스의 완전한 알약형 대신 14px로 둔다. 이름 · 생년월일 · 전공이 세로로 쌓일 때 알약형보다 칸이 또렷하게 구분된다.

### Shadows & Elevation

| Name | Value | Token | 쓰는 곳 |
|------|-------|-------|---|
| hairline | `0 0 0 1px #e6e6e8` | `--shadow-hairline` | 흰 바탕 위 흰 결과 카드 경계 |
| outline | `0 0 0 1px #86868b` | `--shadow-outline` | 바깥선 버튼 |
| toast | `0 8px 24px rgb(29 29 31 / 0.16)` | `--shadow-toast` | **토스트에만** 허용하는 유일한 그림자 |

깊이는 그림자가 아니라 **흰색 ↔ Moon Mist 바탕 차이, 28px 모서리, 1px 선**으로 낸다. 결과 카드 · 타로 카드 · 안내 박스에 그림자 금지.

### Motion

| Name | Value | Token | 쓰는 곳 |
|---|---|---|---|
| card-flip | 700ms `cubic-bezier(0.2, 0.7, 0.2, 1)` | `--animate-card-flip` | 카드 뒤집기 (F-3: 1초 이내) |
| fade-up | 240ms ease-out | `--animate-fade-up` | 해석 3줄이 카드 뒤집기 뒤 나타남 |
| toast-in | 200ms ease-out | `--animate-toast-in` | 토스트 등장 |

- `prefers-reduced-motion: reduce`이면 뒤집기 없이 앞면을 바로 보여 준다.
- 화면 전환에 슬라이드 · 페이드 효과를 넣지 않는다 (1분 목표).

---

## 5. Components

> 파일 위치는 shadcn/ui 관례(`components/ui/*`)를 따른다. 변형은 CVA로, 클래스 합치기는 `cn()`으로.

### 5-1. Button

| variant | 모양 | 쓰는 곳 |
|---|---|---|
| `primary` | `bg-primary text-primary-foreground`, 눌림 `bg-primary-hover` | [타로 시작] · [카드 한 장 뽑기] — 화면당 1개 |
| `outline` | 투명 채움, `text-foreground`, 1px `border-input` | S-03 [이미지 저장] · [결과 공유] |
| `secondary` | `bg-secondary text-secondary-foreground`, 테두리 없음 | S-03 [다시 뽑기] · [처음으로] |
| `ghost` | 투명, `text-muted-foreground` | 상단 ← 뒤로 |
| `link` | `text-accent-foreground` 밑줄 없음, 눌림 시 밑줄 | 대체 화면 [닫기] |

| size | 높이 | 글씨 | 쓰는 곳 |
|---|---|---|---|
| `lg` | 56px, 가로 꽉 참 | `button` 17px/600 | 주요 버튼 (하단 고정) |
| `md` | 48px | `label` 14px/600 → 15px | S-03 2×2 버튼 |
| `icon` | 44×44px | — | ← 뒤로 |

**상태**
- 비활성: `bg-disabled text-steel`, `aria-disabled="true"`. 투명도(opacity)로 흐리게 하지 않는다 — 흐린 보라는 "눌러도 되나?" 헷갈린다.
- 포커스: `focus-visible:ring-2 ring-ring ring-offset-2`.
- 처리 중(이미지 만드는 중): 글씨를 "만드는 중…"으로 바꾸고 비활성. 스피너 아이콘 없음.

```ts
// components/ui/button.tsx
const buttonVariants = cva(
  'inline-flex items-center justify-center gap-1 whitespace-nowrap rounded-full font-semibold transition-colors ' +
  'focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 ' +
  'aria-disabled:pointer-events-none aria-disabled:bg-disabled aria-disabled:text-steel aria-disabled:shadow-none',
  {
    variants: {
      variant: {
        primary:   'bg-primary text-primary-foreground active:bg-primary-hover',
        outline:   'bg-transparent text-foreground shadow-outline active:bg-secondary',
        secondary: 'bg-secondary text-secondary-foreground active:bg-disabled',
        ghost:     'bg-transparent text-muted-foreground active:bg-secondary',
        link:      'bg-transparent text-accent-foreground underline-offset-4 active:underline',
      },
      size: {
        lg:   'h-14 w-full px-6 text-button',
        md:   'h-12 px-5 text-[length:--text-label]',
        icon: 'size-11',
      },
    },
    defaultVariants: { variant: 'primary', size: 'lg' },
  }
)
```

### 5-2. Input (이름 · 전공)

- 높이 52px, `rounded-input`(14px), 바탕 `bg-background`, 1px `border-border`, 안쪽 좌우 16px, 글씨 `body` 16px (16px 미만이면 iOS가 화면을 확대함).
- 포커스: 테두리 `border-primary` + `ring-2 ring-ring/20`.
- 라벨은 칸 위에 `label` 14px/600 Ink. 선택 항목은 라벨 옆에 "(선택)"을 `text-muted-foreground`로.
- 이름: `maxLength={10}`, 오른쪽 아래 `0/10` 글자 수(`caption`, Slate). placeholder "닉네임도 괜찮아요".
- 전공: placeholder "예: 경영학과". 계열을 못 맞춰도 오류를 띄우지 않는다 (해석 3줄째만 빠짐).
- `autocomplete="off"` — 브라우저가 이름을 저장하지 않게 (PRD 원칙 1).

### 5-3. Date Input (생년월일)

- 연 · 월 · 일 숫자 세 칸 (`inputmode="numeric"`), 칸 사이 "." 구분. 모양은 Input과 같고 한 줄에 4 : 3 : 3 비율.
- 실제 없는 날짜(2월 30일 등) · 미래 날짜면 칸 테두리 `border-destructive`, 아래에 `notice` 13px `text-destructive` "날짜를 다시 확인해 주세요" (`role="alert"`).
- 오류는 칸을 벗어날 때(blur) 보여 준다. 입력 중에는 띄우지 않는다.
- 별자리는 입력 화면에 보여 주지 않는다 (결과 화면에서 처음 공개).

### 5-4. Topic Chip Group (주제 6개)

- shadcn `ToggleGroup type="single"` 패턴, `role="radiogroup"`.
- 3열 × 2줄 격자, 칩 사이 8px. 칩 높이 48px, `rounded-full`, `label` 14px/600, 이모지 + 글자.
- 기본: `bg-background`, 1px `border-control-gray`, Ink 글씨.
- 선택: `bg-accent text-accent-foreground`, 1px `border-primary`, 이모지 앞에 체크 없음 (색 변화만으로도 충분 + `aria-checked`).
- 순서 고정: 💼 커리어 · 💰 재물 · 💕 연애 / 🌿 건강 · 📚 학업 · ✨ 종합.
- 주제 묶음은 Moon Mist 밴드(`bg-muted rounded-card p-5`) 안에 둔다 — 입력칸 영역과 시각적으로 나뉨.

### 5-5. Privacy Notice (개인정보 안내)

- `bg-muted rounded-card px-5 py-4`, 🔒 + `notice` 13px `text-muted-foreground`.
- 문구는 기능명세서 원문 그대로:
  > 입력한 이름 · 생년월일 · 전공은 해석을 만드는 데만 쓰고, 서버로 보내거나 저장하지 않아요. 창을 닫거나 [처음으로]를 누르면 바로 지워져요.
- S-02: 입력칸 바로 아래. S-03: 버튼 4개 아래 (짧은 형: "🔒 정보는 저장되지 않아요. [처음으로]를 누르면 바로 지워져요.").
- 접기 · 닫기 버튼 없음.

### 5-6. Tarot Card (카드 그림)

- 비율 **7 : 12** (실제 타로 카드 비율), 결과 카드 안에서 폭 200px (`w-50`), `rounded-tarot`(14px).
- **앞면:** 카드 그림 + 바깥 2px `border-gold` 테두리 + 안쪽 6px 흰 여백. 그림 위에 글씨를 겹치지 않는다.
- **뒷면 (S-01, 뒤집기 전):** `bg-accent` 바탕 + Moon Gold 선으로 그린 단순한 달 · 별 문양 (직접 만든 SVG). 그라디언트 · 반짝이 효과 없음.
- **뒤집기:** `[카드 한 장 뽑기]` → S-03 진입 시 뒷면 → 앞면 Y축 180° 회전 700ms (`perspective: 1000px`, `backface-visibility: hidden`). 끝난 뒤 해석 3줄이 `fade-up`으로 나타난다.
- 정방향만 (역방향 회전 없음).
- `alt="XIX 태양 카드"` 처럼 카드 번호 + 이름.
- 그림 출처: 직접 만든 그림 또는 사용 허가가 분명한 그림만 (PRD 리스크 · 미정).

### 5-7. Result Card (결과 카드 = 저장 이미지 영역)

와이어프레임 S-03의 안쪽 박스. **이 영역이 그대로 1080×1350 PNG가 된다.**

| 순서 | 내용 | 스타일 |
|---|---|---|
| 1 | 지민님의 타로 · 커리어 | `kicker` 15px/600, Ink, 가운데 |
| 2 | 카드 그림 | Tarot Card, 가운데, 위아래 20px |
| 3 | XIX 태양 · ♌ 사자자리 | `card-name` 22px Noto Serif KR, 번호 · 기호는 `text-gold-foreground`. 별자리 기호는 이모지로 바뀌지 않게 글자 모양 선택자를 붙인다 (`♌︎`) — 이모지로 보이면 금색이 적용되지 않는다 |
| 4 | 해석 3줄 | `reading` 17px/500, 가운데 정렬, 줄 사이 4px, `keep-all` |
| 5 | ※ 오락용 타로 해석입니다 | `caption` 12px, Slate |
| 6 | 오늘의 타로 (서비스 이름) | `caption` 12px/600, Slate — **이미지에서만** 하단에 표시 |

- 바탕 `bg-card rounded-card p-6`, 흰 화면 위에서는 `shadow-hairline`으로만 경계.
- **화면에서는 높이를 내용에 맞춘다** (폭 343px, 높이 약 600px). 카드 그림(200×343)과 해석 3줄이 343px 폭의 4 : 5(약 429px) 안에 들어가지 않기 때문이다.
- **4 : 5 비율은 저장 이미지(1080×1350)에만** 적용한다. 같은 내용 · 순서 · 색을 저장 이미지 레이아웃(7장)으로 다시 배치하므로, 화면과 이미지는 "같은 내용"이지 "같은 픽셀"이 아니다.
- **넣지 않는 것:** 생년월일, 전공 원문, 개인정보 안내 박스, 버튼 (PRD 원칙 2).
- 이름 길이 10자 · 해석 90자에서 넘치지 않는지 확인 (가장 긴 문구로 시각 테스트).
- 이미지로 만들 때 화면 폭과 상관없이 1080px 기준으로 다시 그린다 (`html-to-image` 등, `pixelRatio` 조정). 웹폰트가 다 불러진 뒤(`document.fonts.ready`) 찍는다.

### 5-8. Result Actions (S-03 버튼 4개)

- 2 × 2 격자, 간격 12px, 모두 `size="md"`.
- 1줄: [이미지 저장] [결과 공유] — `outline`
- 2줄: [다시 뽑기] [처음으로] — `secondary`
- 4개 중 보라로 채운 버튼은 없다 (결과 화면의 주인공은 카드).
- 저장 · 공유 버튼 위에 작은 안내: "이미지와 공유 글에 이름이 들어가요" (`caption`, Slate) — PRD 리스크 "실제 이름이 퍼짐" 대응.

### 5-9. Toast (짧은 알림)

- 화면(보이는 영역) 하단 중앙, 아래 안전 영역 위 16px (하단 고정 버튼이 있는 화면에서는 그 버튼 위 16px). `position: fixed`라 스크롤해도 같은 자리. `bg-foreground text-background rounded-full px-5 py-3`, `notice` 13px → 14px/500, `shadow-toast`.
- 2.5초 뒤 사라짐. `role="status" aria-live="polite"`.
- 문구 (와이어프레임 안내 문구 표):
  - 이미지 저장 성공: "이미지를 저장했어요"
  - 자동 복사 성공: "결과를 복사했어요. 원하는 앱에 붙여 넣어 주세요"

### 5-10. Fallback Sheet (길게 눌러 저장 · 복사)

인앱 브라우저에서 저장 · 복사가 안 될 때 (유저플로우 저장 · 공유).

- 화면 아래에서 올라오는 시트 (shadcn `Drawer` 패턴), 바탕 `bg-paper-frost`, 위쪽 모서리 28px, 손잡이 막대(36×4px `bg-control-gray`).
- **이미지 저장 안 됨:** 만든 PNG를 `rounded-card`로 띄우고 위에 "이미지를 길게 눌러 저장해 주세요" (`body` 16px/600).
- **복사 실패:** 공유 글을 `bg-background rounded-card p-5`, `user-select: all` 박스에 보여 주고 위에 "아래 글을 길게 눌러 복사해 주세요".
- 아래 [닫기] (`link`). 닫으면 S-03 그대로.

### 5-11. Top Bar

- 높이 52px, 바탕 `bg-background`, 아래 1px 선 없음 (스크롤 시에만 `border-border`).
- S-02: 왼쪽 ← (`ghost icon`, `aria-label="처음 화면으로"`), 제목은 화면 본문에 `title`로 둔다.
- S-01 · S-03: 상단 바 없음.

---

## 6. Screens

### S-01 시작 (F-1)
- 바탕 Gallery White, 세로 가운데 정렬.
- 위에서부터: 🔮 + "오늘의 타로" (`hero`) → 카드 뒷면 (폭 160px) → 소개 2줄 (`body`, Slate, 가운데) → 하단 고정 [타로 시작] (`primary lg`) → 그 아래 `caption` "※ 오락용 타로 · 정보는 저장 안 해요".
- 링크로 들어온 사람도 이 화면부터 (링크에 이름 · 결과 없음, PRD 원칙 5).

### S-02 내 정보 · 주제 (F-2)
- 상단 바 ← → `title` "나에 대해 알려 주세요".
- 입력 3개 (이름 · 생년월일 · 전공(선택)), 묶음 사이 16px.
- Moon Mist 밴드 안에 "어떤 주제가 궁금한가요?" (`label`) + 주제 칩 6개.
- Privacy Notice.
- 하단 고정 [카드 한 장 뽑기] — 이름 · 생년월일(올바른 날짜) · 주제가 모두 채워지기 전엔 비활성.
- [다시 뽑기]로 돌아오면 이름 · 생년월일 · 전공은 채워진 채, **주제 선택만 비운** 상태.
- 전체 높이 약 860px(오류 문구가 있으면 약 890px)로 812px 화면보다 길다 → **세로 스크롤 화면**. [카드 한 장 뽑기]는 하단 고정이라 스크롤과 상관없이 항상 보인다.

### S-03 맞춤 해석 결과 (F-4 ~ F-7)
- 바탕 Moon Mist, 그 위에 흰 Result Card (흰 카드가 떠 보이도록 바탕을 한 단계 어둡게).
- Result Card → Result Actions (2×2) → 짧은 Privacy Notice.
- 전체 높이 약 940px → **세로 스크롤 화면**. 첫 화면(812px)에는 결과 카드 전체와 저장 · 공유 버튼 줄까지 보이고, [다시 뽑기] · [처음으로]와 안내는 조금 내려야 보인다. 버튼은 하단 고정하지 않는다 (카드가 주인공).
- [처음으로]: 확인 팝업 없이 바로 모든 입력값을 지우고 S-01로 (지우는 것이 기본 동작이므로 묻지 않는다).

---

## 7. 저장 이미지 · 공유 글

### 저장 이미지 (1080×1350 PNG)
- 화면 Result Card를 그대로 확대하지 않고, 같은 내용을 1080×1350에 맞춰 다시 배치한다 (5-7 참고).
- 바탕 Gallery White, 바깥 여백 64px, 요소 사이 32px, 가운데 정렬. 카드 모서리는 이미지 자체에선 둥글리지 않고 꽉 채운다.
- 크기: 제목 줄 36px/700 · 카드 그림 400×686 · 카드 이름 52px Noto Serif KR · 해석 3줄 40px/500 (줄 높이 150%) · 오락용 안내 28px · 맨 아래 서비스 이름 28px/700. 남는 높이는 해석과 서비스 이름 사이 빈 공간으로.
- 하단 64px 영역에 "오늘의 타로" 서비스 이름 (`caption` 600, Slate).
- 파일 이름: `taro-{주제}-{카드영문이름}.png` (예: `taro-career-the-sun.png`) — 이름 · 날짜를 파일명에 넣지 않는다.

### 공유 글 (와이어프레임 원문)
```
🔮 {이름}님의 타로 · {주제}
{카드 이름} · {별자리}
{해석 3줄}
※ 오락용 타로 해석입니다
나도 한 장 뽑기 → {시작 링크}
```

---

## 8. 접근성 · 모바일

- 모든 터치 대상 44×44px 이상, 버튼 사이 최소 8px.
- 키보드 포커스 링 항상 보이게 (`focus-visible:ring-2 ring-ring`).
- 색만으로 상태를 전하지 않는다: 비활성 버튼은 `aria-disabled`, 선택 칩은 `aria-checked`, 오류는 문구 함께.
- 해석 · 카드 이름은 텍스트로 (이미지 속 글자로 넣지 않음) — 화면 읽기 프로그램이 읽을 수 있게.
- 하단 고정 버튼은 `env(safe-area-inset-bottom)` 만큼 띄운다 (아이폰 홈 막대).
- 가로 스크롤 없음. 320px 폭에서도 주제 칩 3열이 깨지지 않게 칩 글자 `whitespace-nowrap` + 간격 6px까지 축소 허용.

---

## 9. Do's and Don'ts

### Do
- 결과 화면에서 카드 그림을 가장 크게, 주변은 비워 둔다.
- 보라(primary)는 화면당 주요 버튼 1개와 선택 상태에만 쓴다.
- 결과 카드 · 안내 박스 모서리는 28px, 그림자 없이.
- 해석은 17px/500, 가운데 정렬, `keep-all`로 3줄 안에.
- 개인정보 안내와 오락용 안내를 늘 보이게 둔다.
- 색은 시맨틱 토큰(`bg-primary`, `text-muted-foreground`)으로만 쓴다.

### Don't
- 그라디언트 · 반짝이 · 보랏빛 우주 배경 같은 "점술 사이트" 장식을 쓰지 않는다.
- 결과 카드 · 타로 카드 · 버튼에 그림자를 넣지 않는다 (토스트만 예외).
- 금색을 버튼 · 배경 · 글씨 본문에 쓰지 않는다 (카드 장식 전용).
- 비활성 버튼을 흐린 보라(opacity)로 만들지 않는다 — 회색 채움으로.
- 저장 이미지 · 공유 글 · 파일 이름에 생년월일 · 전공 원문을 넣지 않는다.
- "돈을 벌게 됩니다"처럼 단정하는 문구를 화면 어디에도 넣지 않는다.
- 임의 값(`bg-[#5b3fd1]`, `p-[13px]`)을 쓰지 않는다 — 필요하면 `@theme`에 토큰을 추가.

---

## 10. Quick Start — Tailwind v4

```css
/* app/globals.css */
@import "tailwindcss";

@theme {
  /* ── Brand palette ── */
  --color-gallery-white: #ffffff;
  --color-moon-mist: #f5f5f7;
  --color-paper-frost: #fafafc;
  --color-hairline: #d6d6d6;
  --color-control-gray: #e6e6e8;
  --color-ink: #1d1d1f;
  --color-slate: #6e6e73;
  --color-steel: #86868b;
  --color-night-violet: #5b3fd1;
  --color-violet-ink: #4a32b0;
  --color-violet-haze: #efebfc;
  --color-moon-gold: #b8902f;
  --color-gold-ink: #8a6a1f;
  --color-error-red: #c4321f;

  /* ── Semantic (shadcn naming) ── */
  --color-background: var(--color-gallery-white);
  --color-foreground: var(--color-ink);
  --color-primary: var(--color-night-violet);
  --color-primary-foreground: var(--color-gallery-white);
  --color-primary-hover: var(--color-violet-ink);
  --color-secondary: var(--color-moon-mist);
  --color-secondary-foreground: var(--color-ink);
  --color-muted: var(--color-moon-mist);
  --color-muted-foreground: var(--color-slate);
  --color-accent: var(--color-violet-haze);
  --color-accent-foreground: var(--color-violet-ink);
  --color-gold: var(--color-moon-gold);
  --color-gold-foreground: var(--color-gold-ink);
  --color-destructive: var(--color-error-red);
  --color-card: var(--color-gallery-white);
  --color-card-foreground: var(--color-ink);
  --color-border: var(--color-hairline);
  --color-input: var(--color-steel);
  --color-ring: var(--color-night-violet);
  --color-disabled: var(--color-control-gray);

  /* ── Typography ── */
  --font-sans: "Pretendard Variable", Pretendard, "Apple SD Gothic Neo", "Noto Sans KR", system-ui, sans-serif;
  --font-card: "Noto Serif KR", serif;

  --text-caption: 12px;   --text-caption--line-height: 1.33;   --text-caption--letter-spacing: -0.1px;
  --text-notice: 13px;    --text-notice--line-height: 1.45;    --text-notice--letter-spacing: -0.15px;
  --text-label: 14px;     --text-label--line-height: 1.3;      --text-label--letter-spacing: -0.2px;   --text-label--font-weight: 600;
  --text-body: 16px;      --text-body--line-height: 1.5;       --text-body--letter-spacing: -0.25px;
  --text-reading: 17px;   --text-reading--line-height: 1.6;    --text-reading--letter-spacing: -0.3px; --text-reading--font-weight: 500;
  --text-button: 17px;    --text-button--line-height: 1;       --text-button--letter-spacing: -0.2px;  --text-button--font-weight: 600;
  --text-kicker: 15px;    --text-kicker--line-height: 1.2;     --text-kicker--font-weight: 600;
  --text-card-name: 22px; --text-card-name--line-height: 1.2;  --text-card-name--font-weight: 600;
  --text-title: 24px;     --text-title--line-height: 1.3;      --text-title--letter-spacing: -0.02em;  --text-title--font-weight: 700;
  --text-hero: 34px;      --text-hero--line-height: 1.2;       --text-hero--letter-spacing: -0.02em;   --text-hero--font-weight: 700;

  /* ── Spacing & layout ── */
  --spacing: 4px;
  --container-app: 440px;

  /* ── Radius ── */
  --radius-card: 28px;
  --radius-tarot: 14px;
  --radius-input: 14px;
  --radius-pill: 9999px;

  /* ── Shadows (rings only, toast excepted) ── */
  --shadow-hairline: 0 0 0 1px var(--color-control-gray);
  --shadow-outline: 0 0 0 1px var(--color-steel);
  --shadow-toast: 0 8px 24px rgb(29 29 31 / 0.16);

  /* ── Motion ── */
  --animate-card-flip: card-flip 700ms cubic-bezier(0.2, 0.7, 0.2, 1) both;
  --animate-fade-up: fade-up 240ms ease-out both;
  --animate-toast-in: toast-in 200ms ease-out both;

  @keyframes card-flip {
    from { transform: rotateY(180deg); }
    to   { transform: rotateY(0deg); }
  }
  @keyframes fade-up {
    from { opacity: 0; transform: translateY(8px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes toast-in {
    from { opacity: 0; transform: translateY(8px); }
    to   { opacity: 1; transform: translateY(0); }
  }
}

@layer base {
  * { @apply border-border; }
  html { -webkit-text-size-adjust: 100%; }
  body {
    @apply bg-background text-foreground font-sans antialiased;
    word-break: keep-all;
  }
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { animation-duration: 1ms !important; transition-duration: 1ms !important; }
}
```

### shadcn/ui 설정 메모
- `components.json`: `style: "new-york"`, `tailwind.cssVariables: true`, `baseColor: "neutral"` — 생성된 기본 변수는 위 시맨틱 토큰으로 덮어쓴다.
- 쓰는 컴포넌트: `button`, `input`, `label`, `toggle-group`, `drawer`, `sonner`(토스트). 그 밖의 컴포넌트는 MVP에서 추가하지 않는다.
- shadcn 기본 `--radius`(0.625rem)는 쓰지 않고 위 `--radius-card` · `--radius-input` · `--radius-pill`을 쓴다.
- 다크 모드 변수(`.dark`)는 만들지 않는다 (MVP 범위 밖). 나중에 추가할 때는 `@custom-variant dark (&:where(.dark, .dark *));`로.

---

## 11. 미정 · 확인 필요

- 카드 그림 22장 출처 (직접 제작 / 허가된 그림) — 카드 앞면 스타일(선화 · 단색 · 컬러)이 정해지면 금색 테두리 굵기 재검토.
- 서비스 이름 "오늘의 타로" 확정 여부 (와이어프레임 S-01 제목 기준으로 임시 사용).
- Night Violet 실제 화면 확인 후 미세 조정 (흰 글씨 대비 4.5:1 이상 유지).
- 피그마 카드 앞면 태양 그림은 직접 그린 임시 그림 — 22장 최종 그림이 정해지면 교체.
- 공유 글의 시작 링크 주소 (배포 주소가 정해지면 `{시작 링크}` 교체).
