---
name: post
description: Notion 페이지 URL을 받아 Jekyll 블로그 저장소에 게시 요청 파일을 커밋하고, 저장소의 GitHub Action이 원문 그대로 Markdown으로 변환해 게시하게 한다. "/notion-publish:post 노션 URL", "이 노션 문서 블로그에 올려줘", "노션 페이지 깃블로그에 게시", notion.so 링크와 함께 블로그 · 포스트 · 게시 이야기가 나올 때 사용한다. 같은 페이지를 다시 넘기면 기존 글을 갱신한다.
---

# notion-publish : Notion 페이지 게시

변환은 블로그 저장소의 Action (`scripts/notion_to_md.py --queue`) 이 한다. 이 스킬은 **요청 파일 하나를 커밋하는 일**만 한다. Claude가 본문을 직접 변환하지 않는 이유는 원문 보존 때문이다. 수식 · 표 · 코드가 매번 같은 규칙으로 바뀌어야 하고, 내용이 다시 쓰이면 안 된다.

## 인자

- `<notion-url>` (필수) : `https://www.notion.so/...` 형식의 페이지 링크. 여러 개면 각각 요청 파일을 만든다.
- `--blog owner/repo` (선택) : 블로그 저장소.

## 절차

1. **page id 추출.** URL에서 `?` · `#` 앞 경로의 마지막 32자리 hex (또는 하이픈 포함 36자리 UUID) 를 찾아 하이픈을 뺀 소문자로 만든다. 없으면 "Notion 페이지 링크가 아닌 것 같습니다"라고 알리고 멈춘다.

2. **블로그 저장소 정하기.** 순서대로 :
   - `--blog` 인자
   - 현재 작업 중인 프로젝트 저장소의 `.devlog/config.yml` 의 `blog.repo`
   - `references/defaults.md` 의 기본 블로그 저장소를 제안하고 확인받기

3. **사전 확인.** 블로그 저장소에 `.github/workflows/notion-queue.yml` 과 `scripts/notion_to_md.py` 가 있는지 확인한다. 없으면 커밋하지 않고 "블로그 저장소에 Notion 게시 Action이 아직 없습니다. 플러그인 README의 블로그 저장소 설정을 먼저 적용해 주세요"라고 알린다. Notion 커넥터를 쓸 수 있으면 페이지 제목을 가져와 확인 메시지에 넣는다 (선택, 실패해도 진행).

4. **요청 파일 커밋.** 기본 브랜치에 아래 파일을 커밋한다. 같은 이름이 이미 있으면 (처리 대기 중) 덮어쓴다. 방법은 `references/repo-access.md` 를 따른다.
   - 경로 : `_notion_queue/<page-id>.yml`
   - 내용 :
     ```yaml
     url: <원래 URL>
     requested_at: <ISO 8601, Asia/Seoul>
     ```
   - 커밋 메시지 : `chore(notion): request publish <page-id 앞 8자리>`

5. **결과 알림.** 한두 줄로 :
   - 요청을 보냈고, 1 ~ 2분 뒤 Action이 변환 · 게시한다는 것
   - Actions 링크 : `https://github.com/<repo>/actions/workflows/notion-queue.yml`
   - 같은 페이지를 예전에 올린 적이 있으면 (블로그 `_posts` 에서 `notion_id: <page-id>` 검색) "기존 글을 갱신합니다"라고 덧붙인다

## 실패했을 때

Action이 실패하면 GitHub가 메일로 알린다. 가장 흔한 원인은 Notion integration이 그 페이지에 접근 권한이 없는 경우다. 사용자가 실패를 이야기하면 Notion에서 페이지의 연결 (Connections) 에 블로그용 integration을 추가하라고 안내하고, 같은 URL로 다시 실행하게 한다. 실패한 요청 파일은 저장소에 남아 있으므로 다시 실행하면 덮어쓴다.
