# devlog

프로젝트를 Phase 단위로 나누고, 세션마다 결정과 이유를 기록했다가, Phase가 끝나면 Jekyll 블로그에 개발일지 초안을 PR로 올리는 플러그인입니다.

## 설치 (Claude Code)

```
/plugin marketplace add Yeonb0/yeonb0-plugins
/plugin install devlog@yeonb0-plugins
```

## 명령

| 명령 | 하는 일 |
|---|---|
| `/devlog:note [메모] [--phase N]` | 이번 세션의 결정 · 이유 · 막힌 점을 요약해 프로젝트 저장소 `docs/devlog/phase-N/sessions/` 에 저장 (저장 전 미리보기 확인) |
| `/devlog:publish [--phase N]` | Phase N의 커밋 · PR · 계획 문서 · 세션 기록 · 메모로 초안을 써서 블로그 저장소에 PR 생성, Phase 마감 기록 |
| phase-guide (자동) | 프로젝트 시작 시 Phase 분할 제안, 완료 기준 충족 시 publish 제안, 세션 마무리 시 note 제안 |

## 필요한 것

- 프로젝트 저장소와 블로그 저장소에 쓸 수 있는 GitHub 접근 : 로컬 git, `gh` CLI, GitHub 커넥터 중 하나
- 처음 실행하면 프로젝트 저장소에 `.devlog/config.yml` 을 만드는 질문을 합니다. 설정이 저장소 안에 있으므로 다른 컴퓨터 · 계정의 Claude에서도 그대로 동작합니다.

## 만들어지는 파일

```
프로젝트 저장소
  .devlog/config.yml
  docs/devlog/phase-N/sessions/YYYY-MM-DD-<환경>-<요약>.md
  docs/devlog/phase-N/notes.md

블로그 저장소 (PR로만)
  _posts/devlog/YYYY-MM-DD-<project>-phase-N.md
```

## claude.ai 채팅에서 쓰기

claude.ai 채팅에는 플러그인이 설치되지 않습니다. `skills/note`, `skills/publish`, `skills/phase-guide` 폴더를 각각 zip으로 묶어 claude.ai 설정의 스킬 업로드로 올리면 같은 지시로 동작합니다. GitHub 커넥터가 연결되어 있지 않으면 note는 요약을 보여주기만 하고 저장하지 않습니다.

## 계정 지침에 넣을 문구 (선택)

플러그인의 phase-guide 스킬과 SessionStart hook이 같은 역할을 하지만, 플러그인이 없는 claude.ai 채팅까지 맞추려면 계정 지침에도 넣어 두세요.

```
- 프로젝트를 새로 시작하거나 구현 계획을 세울 때는 작업을 Phase (vertical slice 단위) 로 나누어 제안합니다. 각 Phase에는 목표와 완료 기준을 적습니다.
- 대화 중 현재 Phase의 완료 기준이 충족되었다고 판단되면 "Phase N이 끝난 것 같습니다"라고 알리고, /devlog:publish 실행을 제안합니다. 직접 실행하지는 않습니다.
- 의미 있는 결정이 있었던 세션을 마칠 때는 /devlog:note 로 기록할지 묻습니다.
```
