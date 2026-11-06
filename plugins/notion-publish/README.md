# notion-publish

Notion 페이지 URL 하나로 Jekyll 블로그에 글을 게시하는 플러그인입니다. 변환은 블로그 저장소의 GitHub Action이 기존 `notion_to_md.py` 규칙으로 하므로, 수식 박스 · 코드 `{% raw %}` · 이미지 로컬 저장 · 표 · series / step 이 그대로 유지됩니다.

## 설치 (Claude Code)

```
/plugin marketplace add Yeonb0/yeonb0-plugins
/plugin install notion-publish@yeonb0-plugins
```

## 명령

| 명령 | 하는 일 |
|---|---|
| `/notion-publish:post <url> [--blog owner/repo]` | 블로그 저장소에 `_notion_queue/<page-id>.yml` 을 커밋 → Action이 변환해 `main` 에 게시 |

같은 페이지를 다시 넘기면 front matter의 `notion_id` 로 기존 글을 찾아 **같은 파일을 갱신**합니다 (URL 유지).

## 블로그 저장소 설정 (한 번만)

블로그 저장소 루트에 아래 파일이 있어야 합니다. 다른 블로그에 쓰려면 Yeonb0/Yeonb0.github.io 의 같은 경로에서 복사하세요.

| 파일 | 내용 |
|---|---|
| `.github/workflows/notion.yml` | 기존 파일에 concurrency 그룹 3줄만 추가 |
| `.github/workflows/notion-queue.yml` | `_notion_queue/**.yml` push 시 변환 · 게시. `workflow_dispatch` 로 URL 직접 실행도 가능 |
| `scripts/notion_to_md.py` | 기존 스크립트 + `--page` / `--queue` 모드, 속성 이름 설정 분리, `notion_id` 기록과 재게시 갱신, 인라인 수식 · quote · divider · to_do · toggle · bookmark 지원 |
| `_notion_sync/config.yml` | Notion 속성 이름 매핑 (기존 값과 동일) |
| `_notion_queue/.gitkeep` | 요청 폴더 |

- 기존 6시간 주기 DB 동기화 (`notion.yml`) 는 그대로 동작합니다. 두 workflow는 같은 concurrency 그룹이라 동시에 push하지 않습니다.
- 필요한 secret : 기존 `NOTION_TOKEN` (추가 없음). 게시할 페이지는 이 토큰의 integration에 연결되어 있어야 합니다.

## 다른 계정 · 컴퓨터에서 쓰기

토큰은 블로그 저장소 secret에만 있으므로, 플러그인을 설치하고 GitHub 접근 (커넥터, `gh`, 로컬 git 중 하나) 만 있으면 됩니다. 기본 블로그 저장소는 `skills/post/references/defaults.md` 에서 바꿀 수 있습니다.
