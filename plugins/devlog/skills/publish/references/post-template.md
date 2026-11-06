# 개발일지 글 형식

블로그 (Minimal Mistakes Jekyll) 의 기존 글과 같은 front matter 스타일을 따른다. 기존 글 예시 :

```yaml
---
categories:
- Algorithm
date: '2026-07-07T19:43:00.000+09:00'
layout: single
series: 알고리즘 설계와 분석
step: 2
tags:
- 알고리즘
title: '[Algorithm] Divide-and-Conquer'
toc: true
toc_sticky: true
---
```

## 파일

- 경로 : `<blog.posts_dir>/<YYYY-MM-DD>-<project>-phase-<N>.md` (기본 `_posts/devlog/`)
- 날짜 : publish를 실행한 날 (Asia/Seoul)
- 블로그 permalink가 `/:categories/:title/` 이므로 파일 이름의 slug가 URL이 된다. 영문 소문자 · 숫자 · 하이픈만 쓴다.

## front matter

```yaml
---
categories:
- Devlog                      # blog.category
date: '2026-11-06T21:30:00.000+09:00'
layout: single
series: 라멘티드 개발일지        # "<title> 개발일지"
step: 3                       # Phase 번호
tags:                         # blog.tags + 이번 Phase에서 새로 쓴 기술 2 ~ 3개
- React Native
- Spring Boot
title: '[라멘티드] Phase 3 : 지도 탐색 vertical slice'
toc: true
toc_sticky: true
devlog_project: ramented
devlog_phase: 3
---
```

## 본문 구조

독자는 이 프로젝트를 처음 보는 개발자, 그리고 전공이 아닌 사람도 포함한다. 용어는 처음 나올 때 한 줄로 풀어 쓰고, "무엇을 했다"보다 "왜 그렇게 했고 무엇을 배웠다"에 비중을 둔다. 문체는 "~했다" 체의 회고 글.

```markdown
## 이번 Phase의 목표

(phases[N].goal 과 완료 기준을 독자가 이해할 말로. 이전 Phase가 있으면 한 줄 요약으로 이어 줌)

## 계획

(계획 문서에서 이번 Phase에 해당하는 부분. 어떤 선택지를 놓고 고민했는지)

## 결정과 이유

(세션 기록의 "결정", "이유 · 버린 대안"을 주제별로 묶어 서술. 이 글의 중심)

### <결정 1>
### <결정 2>

## 구현

(커밋 · PR을 기능 단위로 묶어 무엇이 바뀌었는지. 핵심 코드 조각은 10줄 이내로, 필요한 곳에만. 코드 블록은 {% raw %} ... {% endraw %} 로 감싼다)

## 막혔던 점

(세션 기록의 "막힌 점". 증상 → 원인 → 해결 순서)

## 결과와 배운 점

(완료 기준 충족 여부, 다음 Phase로 넘기는 것)

---

<sub>이 글은 커밋 · PR · 계획 문서 · 세션 기록을 바탕으로 초안을 만든 뒤 직접 다듬었습니다. 근거 기록 : 커밋 n개, PR n개, 세션 기록 n개, 메모 n개</sub>
```

- 빠진 소스가 있으면 마지막 줄에 `(이번 Phase에는 세션 기록이 없었습니다)` 처럼 적는다.
- 2주 차 (R8) : 같은 `devlog_project` 의 이전 글이 블로그에 있으면 "이전 글" 링크를 "이번 Phase의 목표" 첫 줄에 넣는다. 링크는 `/<category 소문자>/<이전 글 slug>/` 형식.
- 수식은 블로그의 MathJax 규칙대로 인라인 `$...$`, 블록 `$$...$$`.
