# Contributing Guide

이 저장소에서 일하는 규칙이다. 각 절은 담당자가 자기 PR로 쓴다.

| 섹션 | 담당 |
| --- | --- |
| 1. 브랜치 전략 | 유민규 |
| 2. 브랜치 네이밍 규칙 | 유민규 |
| 3. 커밋 메시지 컨벤션 | 신은영 |
| 4. PR 규칙 | 신은영 |
| 5. 코드 리뷰 규칙 | 민영재 |
| 6. 충돌 대응 흐름 | 민영재 |
| 7. 히스토리를 지키는 규칙 | 유민규 |

## 1. 브랜치 전략 (GitHub Flow)

- `main`: 항상 배포 가능한 상태다. 우리 팀에서 "깨지지 않는 상태"란 README와 docs의 링크가 모두 열리고 표와 목록이 깨지지 않은 상태를 말한다.
- `feature/*`: 작업 단위 브랜치다. 작업 하나를 시작할 때 최신 `main`에서 만들고, PR로 병합한 뒤 지운다.
- `main`은 Branch Protection으로 잠가 두었다.
  - PR을 거쳐야만 병합된다 (Require a pull request before merging)
  - 승인이 1명 이상 있어야 한다 (Required approvals: 1)
  - 관리자도 우회할 수 없다 (Do not allow bypassing the above settings)
- 병합 방식은 **Create a merge commit**만 켜 두었다 (Squash·Rebase merge 끔). 히스토리 그래프에 브랜치 갈래와 PR 번호가 그대로 남게 하려고.

**작업 단위를 나누는 기준**

- 이슈 1개 = 브랜치 1개 = PR 1개.
- PR 하나는 목적이 하나다. 서로 다른 작업을 한 PR에 섞지 않는다.
- 리뷰어가 한 번에 읽을 수 있는 크기로 자른다. 더 커지면 이슈를 나눈다.

**우리 팀이 GitHub Flow를 고른 이유** (3줄 이내)

1. 3명이 작은 작업을 자주 합치는 규모라 develop·release 브랜치를 따로 둘 이유가 없다.
2. 모든 변경이 이슈 → 브랜치 → PR → 리뷰를 거치므로 누가 무엇을 왜 바꿨는지 추적된다.
3. 지킬 브랜치가 `main` 하나라 규칙이 단순하고, 충돌도 작은 PR 단위에서 일찍 드러난다.

## 2. 브랜치 네이밍 규칙

형식: `feature/<GitHub 아이디 소문자>-<주제>`

- `<GitHub 아이디 소문자>`: `minkyu01`, `gpffh20`, `yeomin4242`
- `<주제>`: 영어 소문자 2~4단어를 `-`로 잇는다 (kebab-case).
- 되돌리기나 긴급 수정도 같은 형식을 쓴다 (예: `feature/yeomin4242-revert-tmi`). `revert/…`, `hotfix/…` 같은 다른 접두사는 쓰지 않는다.

| 좋은 예 | 나쁜 예 | 이유 |
| --- | --- | --- |
| `feature/minkyu01-team-intro` | `minkyu01` | 접두사와 주제가 없다 |
| `feature/gpffh20-contributing-commit-pr` | `feature/test` | 누가 무엇을 하는지 모른다 |
| `feature/yeomin4242-revert-tmi` | `revert-12-feature/yeomin4242-…` | GitHub Revert 버튼이 자동으로 짓는 이름은 규칙과 다르다 |

만드는 명령:

```bash
git switch main
git pull --ff-only
git switch -c feature/<GitHub 아이디 소문자>-<주제>
```

## 3. 커밋 메시지 컨벤션

(작성 예정 — 신은영)

## 4. PR 규칙

(작성 예정 — 신은영)

## 5. 코드 리뷰 규칙

(작성 예정 — 민영재)

## 6. 충돌 대응 흐름

(작성 예정 — 민영재)

## 7. 히스토리를 지키는 규칙

- `main`을 포함해 다른 사람이 받아 간 브랜치의 히스토리는 고쳐 쓰지 않는다 (`reset`, `rebase`, `commit --amend` 후 강제 push 금지).
- 강제 push(`--force`, `--force-with-lease`)는 하지 않는다. 예외를 두지 않는다. 이미 push한 커밋의 메시지나 내용이 잘못됐으면 고쳐 쓰지 않고, 다음 커밋이나 `git revert`로 바로잡는다.
- push하기 전의 내 커밋만 고친다: 마지막 커밋 메시지는 `git commit --amend`, 커밋을 다시 나누려면 `git reset --soft HEAD~1`.
- 이미 push한 커밋을 되돌릴 때는 `git revert`를 쓴다. `main`에 들어간 커밋이면 최신 `main`에서 `feature/<아이디>-revert-<주제>` 브랜치를 만들어 되돌린 뒤 PR로 올린다.
