# `.devlog/config.yml` 형식과 첫 실행 설정

설정은 **프로젝트 저장소 안**에 둔다. 그래야 CLI · Cowork · claude.ai 어느 Claude에서든, 다른 계정에서든 GitHub 접근만 되면 같은 설정을 읽는다. 플러그인 코드에는 저장소 이름이나 경로를 하드코딩하지 않는다.

## 형식

```yaml
project: ramented            # 영문 slug. 블로그 파일 이름에 쓰임
title: 라멘티드               # 글 제목 · series 에 쓰는 표시 이름
blog:
  repo: Yeonb0/Yeonb0.github.io
  branch: main               # 블로그 기본 브랜치
  posts_dir: _posts/devlog
  category: Devlog
  tags: [React Native, Spring Boot]
sources:
  planning: [docs/planning/**, README.md]   # 계획 문서로 읽을 경로 (glob)
current_phase: 3
phases:
  - id: 1
    title: 프로젝트 골격
    goal: 로그인 없이 지도 화면까지 뜬다
    done:
      - Expo 앱 실행
      - 지도 WebView 표시
    end_commit: 3f2a9c1        # publish가 기록. 다음 Phase 커밋 범위의 시작점
    post_pr: https://github.com/Yeonb0/Yeonb0.github.io/pull/12
  - id: 3
    title: 지도 탐색 vertical slice
    goal: 가게 핀을 누르면 상세가 열린다
    done:
      - 핀 클릭 → 상세 화면 이동
      - API 실패 시 재시도 안내
```

- `phases` 는 phase-guide 스킬이 관리자님 승인을 받은 뒤 기록한다. 비어 있어도 note · publish는 동작한다.
- `end_commit` · `post_pr` 은 publish만 쓴다. 사람이 고칠 필요는 없다.
- `current_phase` 는 publish가 PR을 만든 뒤 1 올린다.

## 첫 실행 설정 (설정 파일이 없을 때)

질문은 한 번에 묶어서 한다. 저장소에서 알 수 있는 값은 미리 채워 제안한다.

1. 프로젝트 slug와 표시 이름 : 저장소 이름에서 추정해 제안 (예 : `ramented` / `라멘티드`)
2. 블로그 저장소 : 기본값 `Yeonb0/Yeonb0.github.io`, 브랜치 `main` 을 제안하되 확인을 받는다
3. 카테고리 · 태그 : 기본값 `Devlog`, 태그는 저장소 언어 · 주요 의존성에서 추정
4. 계획 문서 경로 : `docs/planning/**`, `README.md` 중 실제로 있는 것을 제안
5. 현재 Phase 번호 : 기본값 1

답을 받으면 위 형식으로 `.devlog/config.yml` 을 만들고, 세션 기록과 같은 방법 (repo-access.md) 으로 저장소에 커밋한다. 커밋 메시지 : `chore(devlog): add .devlog/config.yml`
