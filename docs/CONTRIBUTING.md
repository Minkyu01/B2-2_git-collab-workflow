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

모든 PR에는 작성자와 다른 팀원의 실질적인 줄 코멘트를 하나 이상 남긴다. `LGTM`이나 칭찬만으로는 리뷰를 마치거나 승인하지 않는다. 코멘트는 변경한 줄을 근거로 작성하고 다음 말머리 중 하나를 붙인다.

| 말머리 | 쓰는 경우 |
| --- | --- |
| `[질문]` | 의도나 동작을 확인하고 싶을 때 |
| `[제안]` | 더 나은 표현이나 절차를 제안할 때 |
| `[리스크]` | 빠진 조건이나 협업상 위험을 알릴 때 |
| `[개선]` | 문서의 명확성·재현성을 높일 때 |

작성자는 모든 코멘트에 답한다. 반영하기로 한 내용은 수정하고 `fix: 리뷰 반영 - <무엇을>` 커밋을 push한 뒤, 답글에 반영 커밋을 적는다. 반영하지 않는 경우에도 이유를 답글로 설명한다. 코멘트를 모두 처리한 다음 리뷰어가 대화를 Resolve하고 승인한다.

| PR 작성자 | PR 순서 | 리뷰어 |
| --- | --- | --- |
| 유민규 (`Minkyu01`) | PR1 · PR2 · PR3 | 신은영 · 민영재 · 신은영 |
| 신은영 (`gpffh20`) | PR1 · PR2 | 민영재 · 유민규 |
| 민영재 (`yeomin4242`) | PR1 · PR2 · PR3 | 유민규 · 신은영 · 유민규 |

## 6. 충돌 대응 흐름

충돌이 나면 PR 작성자가 팀에 공유하고 자기 feature 브랜치에서 해결한다. 순서는 다음과 같다.

1. `git fetch origin`으로 원격 정보를 갱신하고 `git merge origin/main`으로 최신 main을 합친다.
2. 충돌 파일의 마커를 확인한다. `<<<<<<< HEAD`와 `=======` 사이는 현재 feature 브랜치의 내용이고, `=======`와 `>>>>>>> origin/main` 사이는 합칠 main의 내용이다. 양쪽 변경 의도를 확인해 필요한 내용을 남기고 마커를 지운다.
3. `git diff --check`로 작업 트리의 공백 오류를 확인한다. 충돌 해결 내용을 지정해 add/rm한 뒤 `git diff --cached --check`로 스테이징된 diff도 검사하고 merge commit을 만든다.
4. 실제 명령과 출력, 해결 기준을 `docs/conflict-resolution.md`에 기록하고 push한다. PR 리뷰어는 양쪽 의도가 보존됐는지 확인한다.

공유 브랜치의 커밋을 바꾸지 않도록 rebase 대신 merge를 사용한다. merge는 양쪽 히스토리를 보존하고, 팀원이 이미 받은 커밋을 다시 쓰거나 강제 push할 필요가 없다. 상대 변경을 버려야 한다면 먼저 상대 PR 작성자와 합의하고 그 이유를 기록한다.

## 7. 히스토리를 지키는 규칙

- `main`을 포함해 다른 사람이 받아 간 브랜치의 히스토리는 고쳐 쓰지 않는다 (`reset`, `rebase`, `commit --amend` 후 강제 push 금지).
- 강제 push(`--force`, `--force-with-lease`)는 하지 않는다. 예외를 두지 않는다. 이미 push한 커밋의 메시지나 내용이 잘못됐으면 고쳐 쓰지 않고, 다음 커밋이나 `git revert`로 바로잡는다.
- push하기 전의 내 커밋만 고친다: 마지막 커밋 메시지는 `git commit --amend`, 커밋을 다시 나누려면 `git reset --soft HEAD~1`.
- 이미 push한 커밋을 되돌릴 때는 `git revert`를 쓴다. `main`에 들어간 커밋이면 최신 `main`에서 `feature/<아이디>-revert-<주제>` 브랜치를 만들어 되돌린 뒤 PR로 올린다.
