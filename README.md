# yeonb0-plugins

Yeonb0의 Claude 플러그인 모음입니다.

| 플러그인 | 하는 일 |
|---|---|
| `devlog` | `/devlog:note`, `/devlog:publish`, phase-guide : 세션 기록 → Phase 개발일지 초안 PR |
| `notion-publish` | `/notion-publish:post` : Notion 페이지 URL → 블로그 게시 |

## 설치 (Claude Code)

```
/plugin marketplace add Yeonb0/yeonb0-plugins
/plugin install devlog@yeonb0-plugins
/plugin install notion-publish@yeonb0-plugins
```

업데이트는 `/plugin marketplace update yeonb0-plugins`.

## 필요한 것

- GitHub 접근 (`gh auth login`, git 인증, 또는 GitHub 커넥터)
- 블로그 저장소에 Notion 게시 Action이 설정되어 있어야 함 (`notion-publish` README 참고)
