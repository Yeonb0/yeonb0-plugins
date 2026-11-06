# GitHub 저장소에 읽고 쓰는 방법

이 플러그인은 CLI · Cowork · claude.ai 어디서든 같은 결과를 내야 한다. 환경마다 쓸 수 있는 도구가 다르므로, 아래 순서대로 **처음으로 동작하는 방법**을 고른다. 쓸 수 없는 방법은 조용히 건너뛰고, 모두 안 되면 무엇이 필요한지 한 문장으로 안내한다.

## 1. 로컬 작업 트리 (Claude Code CLI, 또는 Cowork에 연결된 폴더)

- 현재 작업 디렉터리 (또는 연결된 폴더) 가 대상 저장소의 git 작업 트리이면 이 방법을 쓴다. `git remote get-url origin` 으로 `owner/repo` 를 확인한다.
- 쓰기 : 파일을 만들고 `git add <파일> && git commit -m "<메시지>" && git push`.
- 다른 변경사항이 스테이징되어 있으면 건드리지 않는다. 지정한 파일만 add한다.
- push가 거부되면 (원격이 앞서 있음) `git pull --rebase` 후 한 번만 다시 시도한다.

## 2. `gh` CLI

- `gh auth status` 가 성공하면 쓸 수 있다.
- 파일 쓰기 : `gh api -X PUT repos/<owner>/<repo>/contents/<path>` (본문은 base64, 기존 파일이면 `sha` 포함).
- PR : 블로그 저장소를 임시 디렉터리에 `gh repo clone <repo> -- --depth 1` 로 받아 브랜치 → 커밋 → `git push -u origin <branch>` → `gh pr create`.

## 3. GitHub 커넥터 (MCP 도구 `mcp__GitHub__*` 등)

도구가 지연 로드 상태면 먼저 불러온다. 쓰는 도구 :

| 작업 | 도구 |
|---|---|
| 파일 · 폴더 읽기 | `get_file_contents` |
| 커밋 목록 | `list_commits` (`sha` 에 브랜치, 페이지를 넘기며 `end_commit` 을 만날 때까지) |
| 커밋 상세 | `get_commit` |
| merge된 PR | `search_pull_requests` (`repo:<owner>/<repo> is:merged merged:>=<날짜>`) |
| 파일 하나 쓰기 | `create_or_update_file` (기존 파일이면 `get_file_contents` 로 받은 `sha` 필요) |
| 여러 파일 한 커밋 | `push_files` |
| 브랜치 · PR | `create_branch` → `push_files` → `create_pull_request` |

## 공통 규칙

- 이미 push한 커밋을 amend하거나 force-push하지 않는다. 고칠 것이 있으면 새 커밋을 쌓는다.
- 블로그 저장소의 기본 브랜치에는 직접 쓰지 않는다. 개발일지는 항상 브랜치 + PR로 올린다 (게시 전 검토).
- 프로젝트 저장소의 `docs/devlog/` 와 `.devlog/config.yml` 은 기본 브랜치에 바로 커밋해도 된다. 블로그에 나가는 내용이 아니기 때문이다.
- 어떤 방법을 썼는지는 사용자에게 말하지 않아도 된다. 결과 (파일 경로, PR 링크) 만 알린다.
