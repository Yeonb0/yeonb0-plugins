---
name: publish
description: 끝난 Phase의 커밋 · PR · 계획 문서 · 세션 기록 · 메모를 모아 "계획 → 결정 이유 → 구현 → 결과" 구조의 개발일지 초안을 쓰고, Jekyll 블로그 저장소에 PR로 올린다. "/devlog:publish", "Phase 끝났어 개발일지 써줘", "이번 페이즈 블로그에 올려", "개발일지 초안 PR 만들어줘" 같은 요청이나, Phase 완료 알림에 사용자가 동의했을 때 사용한다.
---

# devlog : Phase 개발일지 게시

한 Phase 동안 흩어져 쌓인 기록을 하나의 회고 글로 묶는다. 게시는 사람이 PR을 읽고 merge할 때 일어나므로, 이 스킬은 초안 PR까지만 만든다.

## 인자

- `--phase N` : 게시할 Phase. 없으면 설정의 `current_phase`.

## 절차

1. **설정 읽기.** 프로젝트 저장소의 `.devlog/config.yml` 을 읽는다. 없으면 `references/config.md` 의 첫 실행 설정을 진행한다. 저장소 접근 방법은 `references/repo-access.md` 를 따른다.

2. **커밋 범위 정하기.** 시작점은 직전 Phase (`id` < N 중 가장 큰 것) 의 `end_commit`. 없으면 저장소의 첫 커밋부터. 끝점은 기본 브랜치의 현재 HEAD. 끝점 SHA를 기억해 둔다 (6단계에서 기록).

3. **소스 수집.** 넷 중 있는 것만 모은다.
   - 커밋 · PR : 범위 안 커밋 메시지, merge된 PR 제목 · 본문. 커밋이 많으면 기능 단위로 묶어 요약한다
   - 계획 문서 : `sources.planning` 경로의 파일 중 이번 Phase와 관련된 부분
   - 세션 기록 : `docs/devlog/phase-<N>/sessions/*.md` 전부
   - 메모 : `docs/devlog/phase-<N>/notes.md`

   수집한 근거로 `phases` 의 id N 항목의 `done` 기준을 하나씩 확인한다. 충족 근거가 없는 기준이 있으면 초안을 쓰기 전에 사용자에게 알리고 "그래도 Phase N을 마감할까요, 남은 기준을 다음 Phase로 넘길까요?"라고 묻는다. 넘기기로 하면 글의 "결과와 배운 점"에 그 사실을 적는다.

   넷 다 비어 있으면 멈추고 "Phase N에 해당하는 기록이 없습니다. 커밋 범위나 Phase 번호를 확인해 주세요"라고 알린다. 일부만 있으면 계속 진행하고, 빠진 소스는 글 끝에 적는다.

4. **초안 작성.** `references/post-template.md` 의 front matter와 본문 구조를 따른다.
   - "결정과 이유"는 세션 기록에 근거가 있는 내용만 쓴다. 근거가 없는 이유를 추측해서 채우지 않는다. 세션 기록이 없으면 이 섹션을 계획 문서와 PR 본문에서 확인되는 범위로 짧게 쓰고, 그 사실을 밝힌다
   - 세션 기록은 이미 민감 정보를 거른 상태지만, 커밋 · PR 본문에서 온 토큰 · 로컬 경로 · 개인 정보가 없는지 한 번 더 확인한다
   - 관리자님의 블로그 독자는 비전공자도 포함한다. 용어는 처음 나올 때 풀어 쓴다

5. **블로그 PR 만들기.** 블로그 저장소 (`blog.repo`) 에서
   - 브랜치 : `devlog/<project>-phase-<N>`. 이미 있으면 (이전에 만든 PR을 다시 쓰는 경우) 같은 브랜치에 새 커밋으로 덮어쓴다
   - 파일 : `<blog.posts_dir>/<YYYY-MM-DD>-<project>-phase-<N>.md`
   - PR 제목 : `[devlog] <title> Phase <N> : <phase title>`
   - PR 본문 : 근거 기록 개수, 빠진 소스, 검토할 때 볼 점 (추측이 섞였을 수 있는 문단이 있다면 그 위치)
   - 기본 브랜치에 직접 커밋하지 않는다. merge가 곧 게시이기 때문이다

6. **Phase 마감 기록.** PR이 만들어지면 프로젝트 저장소의 `.devlog/config.yml` 에서
   - `phases` 의 id N 항목에 `end_commit` (2단계의 끝점) 과 `post_pr` (PR URL) 을 쓴다. 항목이 없으면 `id` · `title` 만으로 새로 만든다
   - `current_phase` 를 N + 1 로 올린다 (N이 현재 Phase일 때만)
   - 커밋 메시지 : `chore(devlog): close phase <N>`

7. **결과 알림.** PR 링크, 근거 기록 개수, 빠진 소스를 짧게 알린다. 다음 Phase가 `phases` 에 정의되어 있지 않으면 phase-guide 스킬로 다음 Phase를 정할지 묻는다.

## 다시 실행할 때

PR을 닫고 다시 쓰고 싶으면 `/devlog:publish --phase N` 을 다시 실행한다. 2단계의 시작점은 직전 Phase 기준이므로 같은 범위가 다시 잡히고, 5단계는 같은 브랜치를 덮어쓴다. 6단계는 `current_phase` 를 다시 올리지 않는다 (이미 N + 1 이므로).
