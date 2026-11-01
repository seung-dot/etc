# DESIGN.md

이 저장소에서 만드는 모든 화면의 디자인 기준 문서입니다. 화면을 만들기 전에 먼저 읽고, 만든 뒤에는 이 문서와 대조해 점검합니다.
`CLAUDE.md`의 UI 제작 규칙과 충돌하면 `CLAUDE.md`가 우선합니다.

## 0. 전제 (수정 가능)

| 항목 | 현재 가정 | 바뀌면 고칠 곳 |
|---|---|---|
| 용도 | 일반중학교 특수학급 수업 자료와 교사용 화면 | 2장, 3장 |
| 주 이용자 | 지적장애·자폐성장애 중학생(학생용), 특수교사(교사용) | 6장 |
| 기술 스택 | HTML과 CSS 중심. 필요한 경우에만 최소한의 JavaScript | 8장 |
| 화면 환경 | 교실 모니터, 태블릿, 인쇄물(A4) | 4장 |

## 1. 방향

**스타일: 차분한 에디토리얼 (Calm Editorial)**

- 종이 질감의 따뜻한 바탕 위에 큰 제목과 작은 본문의 대비로 위계를 만듭니다.
- 장식은 줄입니다. 선, 여백, 글자 크기로만 구조를 보여 줍니다.
- 학생이 "지금 무엇을 하는지"를 한눈에 알 수 있어야 합니다. 화면 하나에는 과제 하나만 둡니다.
- 예측 가능성을 우선합니다. 같은 기능은 항상 같은 위치, 같은 모양으로 둡니다.
- 비대칭 배치는 제목 영역에만 사용합니다. 학생이 조작하는 영역은 정렬된 단일 열로 둡니다.

## 2. 타이포그래피

시스템 기본 폰트는 쓰지 않습니다. 아래 두 폰트를 Google Fonts에서 불러옵니다.

| 역할 | 폰트 | 굵기 | 사용처 |
|---|---|---|---|
| 제목(Display) | Noto Serif KR | 600, 800 | 화면 제목, 단계 번호, 큰 숫자 |
| 본문(Text) | IBM Plex Sans KR | 400, 500, 600 | 본문, 버튼, 안내문, 표 |

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+KR:wght@400;500;600&family=Noto+Serif+KR:wght@600;800&display=swap" rel="stylesheet">
```

**크기 척도** (기준 본문 20px, 비율 약 1.25~1.5. 모든 값은 4의 배수)

| 토큰 | 크기 | 줄 높이 | 용도 |
|---|---|---|---|
| `--text-display` | 64px | 72px | 화면 대표 제목(학생용 첫 화면) |
| `--text-title` | 40px | 48px | 화면 제목 |
| `--text-heading` | 28px | 40px | 단계 제목 |
| `--text-body` | 20px | 32px | 본문, 안내문 |
| `--text-label` | 16px | 24px | 보조 설명, 캡션(최소 크기) |

- 본문은 20px 이상을 기본으로 합니다. 16px 미만은 쓰지 않습니다.
- 한 줄 길이는 한글 기준 28자 안팎으로 제한합니다(`max-width: 28em`).
- 가운데 정렬은 제목에만 씁니다. 본문은 왼쪽 정렬입니다.
- 이탤릭, 밑줄 장식, 전체 대문자 변환은 쓰지 않습니다. 강조는 굵기로 합니다.

## 3. 색 토큰

색은 아래 토큰으로만 사용합니다. 이 블록 밖에서 헥사값을 직접 쓰지 않습니다.
대비는 계산해서 확인했습니다(WCAG 기준: 본문 4.5 이상, 큰 글자와 경계선 3 이상).

```css
:root {
  /* 바탕 */
  --color-paper: #F6F2EA;        /* 페이지 배경 */
  --color-surface: #FCFAF5;      /* 카드, 입력 영역 */

  /* 글자 */
  --color-ink: #1F1B16;          /* 본문, 제목 (paper 대비 15.3) */
  --color-ink-muted: #5B534A;    /* 보조 글자 (paper 대비 6.8) */

  /* 선 */
  --color-line: #D9D1C3;         /* 장식용 구분선(정보 전달 금지) */
  --color-line-strong: #7A7062;  /* 입력·버튼 경계선 (paper 대비 4.4) */

  /* 강조 */
  --color-accent: #1F4D6B;       /* 주 행동 (paper 대비 8.1) */
  --color-accent-strong: #163A52;/* 눌림, 호버 (paper 대비 10.7) */
  --color-accent-soft: #DCE8EF;  /* 선택된 배경 (ink 대비 13.7) */
  --color-on-accent: #F6F2EA;    /* accent 위의 글자 */

  /* 상태 (색만으로 전달하지 않고 글자와 아이콘을 함께 씀) */
  --color-ok: #2F6B4F;           /* 완료, 정답 (paper 대비 5.6) */
  --color-warn: #8A5A00;         /* 주의 (paper 대비 5.3) */
  --color-danger: #9B2C2C;       /* 오류 (paper 대비 6.7) */

  /* 포커스 */
  --color-focus: #1F4D6B;
}
```

- 한 화면에서 강조색(`--color-accent`)은 "지금 눌러야 하는 것" 하나에만 씁니다.
- 상태는 색 외에 글자("완료", "다시 해 보기")를 반드시 함께 표시합니다.
- 순수 검정(`#000`)과 순수 흰색(`#FFF`)은 쓰지 않습니다.
- 다크 모드는 현재 범위에 포함하지 않습니다. 필요해지면 이 장에 토큰을 추가합니다.

## 4. 간격과 레이아웃

**간격 척도** (4px 기준, 8px 단위 우선)

```css
:root {
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-12: 48px;
  --space-16: 64px;
  --space-24: 96px;

  --radius-sm: 4px;
  --radius-md: 8px;
}
```

- 여백도 크기 대비를 둡니다. 같은 그룹 안은 `--space-2`~`--space-4`, 그룹 사이는 `--space-12` 이상으로 벌립니다.
- 모든 간격을 같게 두지 않습니다. 제목 위 여백은 제목 아래 여백보다 크게 둡니다.
- 터치 대상(버튼, 선택지)은 높이 56px 이상, 서로 간격 16px 이상입니다.
- 화면 가로 최대 폭은 1120px, 좌우 여백은 `--space-6`(모바일) / `--space-12`(데스크톱)입니다.
- 제목 영역은 12열 격자에서 왼쪽 7열에 제목, 오른쪽 4열에 보조 설명을 두는 비대칭 배치를 기본으로 합니다.
- 인쇄(A4)용은 여백 20mm, 본문 14pt 이상, 배경색 없이 선만으로 구조를 만듭니다.

## 5. 모션

```css
:root {
  --ease-out: cubic-bezier(0.23, 1, 0.32, 1);
  --ease-in: cubic-bezier(0.5, 0, 0.75, 0);
  --duration-enter: 240ms;
  --duration-exit: 160ms;   /* 퇴장은 등장의 약 2/3 */
  --duration-press: 80ms;
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

- 애니메이션 속성은 `transform`과 `opacity`만 씁니다.
- 학생용 화면은 감각 부담을 줄이기 위해 모션을 최소로 합니다. 허용: 등장 페이드(`opacity` + `translateY` 8px 이하), 버튼 눌림(`scale(0.98)`).
- 금지: 반복 애니메이션, 깜빡임, 자동 슬라이드, 흔들림, 시차 스크롤.
- 화면 전환은 새 내용이 나타나기 전에 이전 내용이 먼저 사라지게 하지 않고, 같은 위치에서 교체합니다.

## 6. 상호작용 상태

모든 조작 요소는 아래 상태를 모두 가집니다.

| 상태 | 표현 |
|---|---|
| 기본 | `--color-surface` 배경, 2px `--color-line-strong` 경계선 |
| hover | 배경을 `--color-accent-soft`로, 경계선을 `--color-accent`로 |
| focus-visible | 3px `--color-focus` 외곽선, 외곽선 간격 3px. 제거하지 않습니다 |
| active | `transform: scale(0.98)`, 배경을 `--color-accent-strong`으로 |
| 선택됨 | `--color-accent-soft` 배경 + 체크 표시 + "선택함" 글자 |
| 비활성 | `--color-ink-muted` 글자, 점선 경계선, 클릭 불가 표시 |

**학생용 화면 원칙**
- 지시문은 한 문장, 한 동작입니다. 예: "알맞은 것을 고르세요."
- 복잡한 과제는 "1단계 / 2단계" 번호를 큰 글자로 쪼개어 보여 줍니다.
- 선택지(고르기)와 문장틀(빈칸 채우기)을 기본 응답 방식으로 씁니다.
- 마지막에 스스로 확인하는 체크리스트를 둡니다.
- 오류 메시지는 비난하지 않는 표현으로 "다시 해 보세요"처럼 다음 행동을 알려 줍니다.
- 이모티콘, 효과음, 자동 재생은 쓰지 않습니다.

## 7. 컴포넌트 기준

| 컴포넌트 | 기준 |
|---|---|
| 주 버튼 | `--color-accent` 배경, `--color-on-accent` 글자, 높이 56px, 글자 20px 600 |
| 보조 버튼 | 투명 배경, 2px `--color-line-strong` 경계선 |
| 선택 카드 | 전체 영역이 클릭 대상, 최소 높이 64px, 선택 시 체크 표시 |
| 입력 칸 | 높이 56px, 라벨은 항상 위에 표시(placeholder로 대신하지 않음) |
| 단계 표시 | 큰 숫자(Noto Serif KR 800)와 짧은 제목, 현재 단계만 강조 |
| 체크리스트 | 항목마다 큰 체크 칸(32px 이상)과 한 줄 문장 |

## 8. 구현 규칙

- 색, 간격, 모션 값은 위 토큰만 사용하고 직접 숫자를 쓰지 않습니다.
- 마크업은 시맨틱 요소(`header`, `main`, `button`, `label`)를 씁니다. 클릭 가능한 `div`는 쓰지 않습니다.
- 이미지에는 대체 텍스트를 넣습니다. 장식 이미지는 `alt=""`입니다.
- 키보드만으로 모든 조작이 가능해야 하고, 포커스 순서는 화면 읽는 순서와 같아야 합니다.
- `<html lang="ko">`를 지정합니다.

## 9. 완성 후 점검표

화면을 만든 뒤 아래를 하나씩 확인하고, 결과를 보고합니다.

- [ ] 스타일이 하나로 일관되는가 (장식 요소가 섞이지 않았는가)
- [ ] 제목과 본문의 크기 차이가 한눈에 보이는가
- [ ] 이 문서의 토큰 외에 직접 쓴 색·간격 값이 없는가
- [ ] 모든 조작 요소에 hover / focus / active가 있는가
- [ ] 애니메이션이 `transform`과 `opacity`뿐이며, `prefers-reduced-motion`에서 멈추는가
- [ ] 본문 글자 대비가 4.5 이상인가, 글자 크기가 16px 이상인가
- [ ] 화면 하나에 과제 하나만 있는가
- [ ] 실제 브라우저로 열어 스크린샷을 확인했는가 (`agent-browser`)
- [ ] 스크린샷에서 어색한 부분(여백 불균형, 줄바꿈, 잘림)을 찾아 고쳤는가
- [ ] `web-design-guidelines`로 접근성·사용성 검사를 마쳤는가

## 10. 작업 순서

1. 이 문서를 읽는다.
2. `design-taste-frontend`로 방향을 확인하고 화면을 만든다.
3. 모션이 필요하면 `animate`를 참고하되, `CLAUDE.md`의 규칙을 우선한다.
4. `agent-browser`로 실제 화면을 열고 스크린샷을 확인한다.
5. 9장 점검표와 `web-design-guidelines`로 검수하고 수정한다.
