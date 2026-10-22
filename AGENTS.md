# AGENTS.md

이 저장소는 Codyssey B2-2 「실전 Git 협업 워크플로우」 팀 과제 저장소다. 팀원 3명이 GitHub Flow로 협업하고, 그 과정을 GitHub 기록(이슈, PR, 리뷰, 병합)과 문서로 남기는 것이 결과물이다. 코드를 만드는 과제가 아니다.

이 파일은 팀원 각자가 쓰는 AI 에이전트(Codex, Claude Code, Cursor 등)가 읽는 작업 지시서다. 사람이 읽는 안내와 설명 카드는 [docs/team-guide.md](docs/team-guide.md)에 있다.

## 0. 꼭 지킬 것

1. 지금 이 컴퓨터의 사용자 한 명만 돕는다. 사용자는 `gh api user --jq .login`으로 확인한다. 다른 팀원의 차례, 다른 팀원의 이슈·PR·커밋은 건드리지 않는다.
2. 5절 차례표의 순서와 관문을 지킨다. 시작 조건이 안 됐으면 무엇을 기다리는지 말하고 멈춘다.
3. `main`에 직접 커밋하거나 push하지 않는다. 강제 push(`--force`, `--force-with-lease`)를 하지 않는다. 이미 push한 커밋에 `rebase`, `commit --amend`, `reset`을 쓰지 않는다.
4. 실습 기록(`docs/troubleshooting-log.md`, `docs/conflict-resolution.md`)에는 실제로 실행한 명령과 실제 출력만 붙인다. 출력을 지어내거나 고치지 않는다.
5. 자기소개처럼 사용자 개인에 관한 내용은 사용자에게 물어서 쓴다. 지어내지 않는다.
6. 다음 동작은 사용자에게 내용을 보여 주고 허락을 받은 뒤에 한다: 리뷰 코멘트 게시, 승인(Approve), 병합(Merge), 충돌 정리 결과 커밋.
7. 한 단계를 마칠 때마다 사용자에게 세 가지를 짧게 말한다: 방금 한 일, 왜 그렇게 하는지, 평가에서 어떻게 설명하면 되는지([docs/team-guide.md](docs/team-guide.md)의 설명 카드). 그리고 팀 채팅에 보낼 한 줄을 만들어 준다. 채팅은 사용자가 직접 보낸다.
8. PR 본문, 리뷰 JSON 같은 임시 파일은 저장소 밖에 만든다 (예: `mktemp`). 저장소에 커밋하지 않는다.

## 1. 과제 요약 (명세를 우리 말로 정리)

결과물은 팀 GitHub 저장소 1개: `SUBMISSION.md`(제출물 인덱스), `docs/CONTRIBUTING.md`, `docs/conflict-resolution.md`, `docs/troubleshooting-log.md`, `git log --oneline --graph --all` 결과, 간단한 결과물(우리는 팀 소개).

| # | 요구 | 우리 계획에서 |
| --- | --- | --- |
| 1 | 3~5명, 저장소 1개, `README.md`·`docs/`·`src/`·`team/`, `main` 보호(직접 push 금지, PR로만, 승인 1명 이상) | 준비 완료 |
| 2 | GitHub Flow: `main`은 항상 깨지지 않게, 작업은 `feature/*`. 브랜치 이름 규칙과 GitHub Flow를 고른 이유(3줄 이내)를 문서로 | 1단계 (CONTRIBUTING 1·2절) |
| 3 | 작업마다 이슈, PR 본문에 `Closes #번호`. `SUBMISSION.md`에서 팀원별 이슈·PR 확인 | 모든 단계, 13단계 |
| 4 | 커밋 메시지 규칙(`feat:`, `fix:`, `docs:` 등). `update`, `fix`, `temp`, `wip`, `final`만 있거나 무엇을 왜 바꿨는지 없는 메시지(`bug fix`, `edit file`) 금지 | 모든 커밋, 1단계 amend |
| 5 | 모든 feature 브랜치는 PR로 병합. 1인당 PR 생성·병합 2개, 다른 사람 PR 리뷰 2개, 내 PR에서 리뷰 반영 1회 이상. PR 본문에 What·Why·How | PR 8개, 리뷰 순번표 |
| 6 | PR마다 LGTM이 아닌 실질 코멘트 1개 이상, 리뷰어와 작성자가 주고받은 기록 | 모든 리뷰 |
| 7 | 일부러 충돌 2회 이상, 그중 1회 이상 비자명(같은 파일 인접 라인을 서로 다르게 수정, 또는 한쪽 이동·삭제 + 다른 쪽 수정). 기록 | 10·11단계 |
| 8 | `commit --amend`, `reset --soft HEAD~1`, `revert`, `stash`/`stash pop`을 팀이 모두 하고 기록. 1인당 1개 이상 기록 참여 | 1·2·5·12단계 |
| 9 | `docs/CONTRIBUTING.md`를 나눠 씀: 브랜치 이름, 커밋 규칙, PR 규칙, 리뷰 규칙, 충돌 대응 흐름 | 1~3단계 |
| 10 | 간단한 결과물 1개, 1인당 기여 커밋 1건 이상. 우리는 (B) 팀 소개: `team/<아이디>.md` + README 표에서 링크 | 7~11단계 |

제약: 공유 브랜치 히스토리 재작성 금지, 합의 없는 강제 push·rebase 금지, 기록은 상황·절차·결과·주의점이 있어 다시 해 볼 수 있어야 한다.

## 2. 팀

| 이름 | GitHub | 브랜치 앞부분 | CONTRIBUTING 담당 | 실습 |
| --- | --- | --- | --- | --- |
| 유민규 | `Minkyu01` | `feature/minkyu01-` | 1·2·7절 | amend, 충돌 #2 해결 |
| 신은영 | `gpffh20` | `feature/gpffh20-` | 3·4절 | reset, stash |
| 민영재 | `yeomin4242` | `feature/yeomin4242-` | 5·6절 | revert, 충돌 #1 해결 |

리뷰 순번표 (PR 작성자 → 리뷰어):

| PR | 리뷰어 |
| --- | --- |
| 민규 PR1 · PR2 · PR3 | 신은영 · 민영재 · 신은영 |
| 은영 PR1 · PR2 | 민영재 · 유민규 |
| 영재 PR1 · PR2 · PR3 | 유민규 · 신은영 · 유민규 |

## 3. 공통 규칙

- 브랜치: `feature/<GitHub 아이디 소문자>-<주제>`. 항상 최신 `main`에서 만든다 (2단계의 은영 PR2만 예외, 5절 참고).
- 커밋: `<type>: <무엇을>` (type: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`). Git이 만드는 병합 커밋(`Merge …`)과 revert 커밋(`Revert "…"`)의 기본 메시지는 그대로 둔다.
- 이슈 먼저: `gh issue create --title "<커밋 규칙과 같은 제목>" --body "<할 일 한두 줄>" --assignee @me`
- PR: 제목은 커밋 규칙과 같게. 본문은 `.github/pull_request_template.md`의 칸을 모두 채운다(연결 이슈 `Closes #번호`, What, Why, How, 리뷰어에게). 리뷰어는 순번표대로 `--reviewer <아이디>`.
- 리뷰: Files changed의 특정 줄에 코멘트 1개 이상. 말머리 `[질문]` `[제안]` `[리스크]` `[개선]`. 칭찬만 있는 코멘트나 LGTM만으로 승인하지 않는다.
- 반영: 코멘트마다 답글. 고쳤으면 `fix: 리뷰 반영 - <무엇을>` 커밋을 push하고 답글에 커밋 해시를 적는다.
- 병합: 승인 1명 이상 + 모든 코멘트에 답글 + 차례표 관문 충족. `gh pr merge <번호> --merge --delete-branch`. 저장소는 merge commit만 허용한다.
- 충돌: 자기 feature 브랜치에서 `git fetch origin && git merge origin/main`으로 푼다. rebase하지 않는다.
- 두 기록 문서 맨 위의 요약 표는 건드리지 않는다. PR 번호는 13단계에서 유민규가 채운다. 각자 자기 절만 채운다.
- 커밋 작성자 이메일은 GitHub noreply 주소로 한다 (6절 0단계).
- `git add .`를 쓰지 않고 파일을 지정해서 add한다. 예외는 2단계 reset 실습 하나다. AI 도구나 편집기가 만드는 파일(`.claude/`, `.cursor/`, `.DS_Store` 등)은 `.gitignore`에 들어 있다.

## 4. 시작할 때마다

```bash
gh api user --jq .login                      # 나는 누구인가
git fetch origin --prune
git status --short --branch                  # 하던 작업이 남아 있는가
gh pr list --state all --limit 50 --json number,title,headRefName,state,reviewDecision,author
```

5절의 "내 할 일 순서"에서 아직 끝나지 않은 가장 앞 단계를 찾고, 그 단계의 시작 조건이 됐는지 PR 목록으로 확인한다. 됐으면 6절의 그 단계를 진행하고, 안 됐으면 무엇을 기다리는지 사용자에게 말한다.

## 5. 차례표

| 단계 | 누가 | 할 일 | 시작 조건 | 끝 표시 |
| --- | --- | --- | --- | --- |
| 0 | 모두 | 합류: clone, 커밋 이메일, gh 로그인 | 초대 수락 | 팀 채팅에 "준비 완료" (셋 다 = **관문 A**) |
| 1 | 유민규 | PR1 열기: CONTRIBUTING 1·2·7절 + amend 실습 | 관문 A | `feature/minkyu01-contributing-flow` PR 열림 |
| 2 | 신은영 | PR1 열기: 3·4절 + reset 실습. 이어서 PR2를 커밋하지 않고 시작만 | 관문 A | `feature/gpffh20-contributing-commit-pr` PR 열림 |
| 3 | 민영재 | PR1 열기: 5·6절 | 관문 A | `feature/yeomin4242-contributing-review` PR 열림 |
| 4 | 리뷰어 셋 | PR1 리뷰: 줄 코멘트 (승인은 아직) | 리뷰할 PR이 열림 | 코멘트 게시 |
| 5 | 작성자 셋 | 반영 + 답글. 은영은 stash 실습으로 반영 | 내 PR에 코멘트 | 모든 코멘트에 답글 |
| 6 | 모두 | 승인 → 작성자가 병합 ①②③ (순서 무관) | 답글 완료 | PR1 셋 다 MERGED = **관문 B** |
| 7 | 민영재 | PR2 열기 (병합 금지) | 관문 B | `feature/yeomin4242-team-intro` PR 열림 |
| 8 | 유민규 | PR2 열기 (병합 금지) | 관문 B | `feature/minkyu01-team-intro` PR 열림 (7·8 둘 다 = **관문 C**) |
| 9 | 신은영 (리뷰 유민규) | PR2 열기 → 리뷰·반영·승인 → 병합 ④ | 관문 C | `feature/gpffh20-team-intro` MERGED |
| 10 | 민영재 (리뷰 신은영) | main 받기 → 충돌 #1 해결·기록 → 리뷰·반영·승인 → 병합 ⑤ | 9단계 끝 | `feature/yeomin4242-team-intro` MERGED |
| 11 | 유민규 (리뷰 민영재) | main 받기 → 충돌 #2 해결·기록 → 리뷰·반영·승인 → 병합 ⑥ | 10단계 끝 | `feature/minkyu01-team-intro` MERGED |
| 12 | 민영재 (리뷰 유민규) | TMI 커밋 revert PR → 기록 → … → 병합 ⑦ | 11단계 끝 | `feature/yeomin4242-revert-tmi` MERGED |
| 13 | 유민규 (리뷰 신은영) | SUBMISSION → … → 병합 ⑧ → 최종 그래프 코멘트 | 12단계 끝 | `feature/minkyu01-submission` MERGED |
| 14 | 모두 | 설명 연습 | 13단계 끝 | — |

PR2 병합 순서(④ 은영 → ⑤ 영재 → ⑥ 민규)는 바꾸면 안 된다. 이 순서여야 충돌 #1, #2가 계획한 곳에서 난다.

내 할 일 순서:

- 유민규: 1 → 4 (영재 PR1 리뷰) → 5 → 6 (영재 PR1 승인, 내 PR1 병합) → 8 → 9 (은영 PR2 리뷰·승인) → 11 → 12 (영재 PR3 리뷰·승인) → 13
- 신은영: 2 → 4 (민규 PR1 리뷰) → 5 (stash) → 6 (민규 PR1 승인, 내 PR1 병합) → 9 → 10 (영재 PR2 리뷰·승인) → 13 (민규 PR3 리뷰·승인)
- 민영재: 3 → 4 (은영 PR1 리뷰) → 5 → 6 (은영 PR1 승인, 내 PR1 병합) → 7 → 10 → 11 (민규 PR2 리뷰·승인) → 12

## 6. 단계별 할 일

### 0단계 · 합류 (모두)

```bash
git clone https://github.com/Minkyu01/B2-2_git-collab-workflow.git
cd B2-2_git-collab-workflow
gh auth status                               # 로그인이 안 되어 있으면 사용자가 직접 gh auth login
git config user.name "$(gh api user --jq .login)"
git config user.email "$(gh api user --jq '"\(.id)+\(.login)@users.noreply.github.com"')"
git --version                                # 출력은 기록의 '공통 환경'에 쓰인다
```

팀 채팅 한 줄: `[0단계] <이름> 준비 완료`

### 1단계 · 유민규 PR1 (CONTRIBUTING 1·2·7절 + amend)

```bash
git switch main && git pull --ff-only
gh issue create --title "docs: CONTRIBUTING 1·2·7절 작성" --body "브랜치 전략, 브랜치 이름 규칙, 히스토리 규칙 절을 쓴다." --assignee @me
git switch -c feature/minkyu01-contributing-flow
```

1. `docs/CONTRIBUTING.md`의 1·2·7절을 7절의 "절별 내용"대로 쓴다.
2. amend 실습: 일부러 규칙을 어긴 메시지로 커밋하고, push하기 전에 고친다. 두 출력은 그대로 저장한다.
   ```bash
   git add docs/CONTRIBUTING.md
   git commit -m "docs: update"
   git log --oneline -1                      # 전
   git commit --amend -m "docs: CONTRIBUTING 1·2·7절 작성"
   git log --oneline -1                      # 후 (해시가 바뀐다)
   ```
3. `docs/troubleshooting-log.md`의 "시나리오: amend" 절을 채운다 (8절 형식). `git commit -am "docs: amend 실습 기록 추가"`
4. `git push -u origin HEAD` → PR 생성, 리뷰어 `gpffh20`.

팀 채팅 한 줄: `[1단계] 민규 PR1 열었어요 (#번호). 은영 리뷰 부탁해요.`

### 2단계 · 신은영 PR1 (3·4절 + reset) + PR2 시작

```bash
git switch main && git pull --ff-only
gh issue create --title "docs: CONTRIBUTING 3·4절 작성" --body "커밋 메시지 규칙과 PR 규칙 절을 쓴다." --assignee @me
git switch -c feature/gpffh20-contributing-commit-pr
```

1. `docs/CONTRIBUTING.md`의 3·4절을 쓴다.
2. reset 실습: 개인 메모 `todo.txt`까지 일부러 같이 커밋한 뒤 되돌린다.
   ```bash
   echo "개인 메모" > todo.txt
   git add .
   git commit -m "docs: CONTRIBUTING 3·4절 작성"
   git show --name-only --format= HEAD      # 전: todo.txt가 들어 있다
   git reset --soft HEAD~1
   git restore --staged todo.txt
   git commit -m "docs: CONTRIBUTING 3·4절 작성"
   git show --name-only --format= HEAD      # 후: CONTRIBUTING.md만
   rm todo.txt
   ```
3. "시나리오: reset" 절을 채우고 `git commit -am "docs: reset 실습 기록 추가"` → push → PR 생성, 리뷰어 `yeomin4242`.
4. PR2 시작 (stash 실습 준비). PR2 이슈를 만들고, 아직 아무도 병합하지 않은 지금의 `main`에서 브랜치를 만든다.
   ```bash
   gh issue create --title "docs: 신은영 자기소개 추가" --body "team/gpffh20.md를 쓰고 README 팀원 표에 링크한다." --assignee @me
   git switch main
   git switch -c feature/gpffh20-team-intro
   ```
   `team/gpffh20.md`를 `team/README.md` 형식으로 쓰기 시작하고(내용은 사용자에게 묻는다), `README.md` 팀원 표의 신은영 줄 `(작성 예정)`을 `[소개](team/gpffh20.md)`로 바꾼다. **커밋하지 않고 그대로 둔다.**

팀 채팅 한 줄: `[2단계] 은영 PR1 열었어요 (#번호). 영재 리뷰 부탁해요.`

### 3단계 · 민영재 PR1 (5·6절)

```bash
git switch main && git pull --ff-only
gh issue create --title "docs: CONTRIBUTING 5·6절 작성" --body "코드 리뷰 규칙과 충돌 대응 흐름 절을 쓴다." --assignee @me
git switch -c feature/yeomin4242-contributing-review
```

5·6절을 쓰고 `git commit -am "docs: CONTRIBUTING 5·6절 작성"` → push → PR 생성, 리뷰어 `Minkyu01`.

팀 채팅 한 줄: `[3단계] 영재 PR1 열었어요 (#번호). 민규 리뷰 부탁해요.`

### 4단계 · PR1 리뷰 (리뷰어)

9절 "리뷰하는 법"대로 줄 코멘트를 1개 이상 단다. 승인은 아직 하지 않는다.

- 유민규 → 영재 PR1, 신은영 → 민규 PR1 (amend 기록을 직접 다시 해 볼 것), 민영재 → 은영 PR1 (reset 기록 확인)

팀 채팅 한 줄: `[4단계] <PR 작성자> PR1에 리뷰 남겼어요.`

### 5단계 · 반영 (작성자)

내 PR의 코멘트를 읽고, 코멘트마다 고치거나 이유를 답한다 (9절).

**신은영은 stash 실습으로 반영한다.** PR2 작업이 커밋되지 않은 채 남아 있는 상태여야 한다.

```bash
git status --short                           # 전: team/gpffh20.md(새 파일), README.md(수정)
git stash push -u -m "PR2 자기소개 작성 중"   # -u: 새 파일도 같이 보관
git stash list
git switch feature/gpffh20-contributing-commit-pr
# 코멘트 반영 → git commit -am "fix: 리뷰 반영 - <무엇을>" → git push → 답글
git switch feature/gpffh20-team-intro
git stash pop
git status --short                           # 후: 작업이 돌아왔다
```

pop한 직후 "시나리오: stash" 절을 위 출력으로 채워 둔다 (PR2에 같이 커밋된다).

팀 채팅 한 줄: `[5단계] PR1 리뷰 반영했어요. 확인 부탁해요.`

### 6단계 · 승인과 병합 (모두)

- 리뷰어: 작성자가 모든 코멘트에 답했는지 보고, 사용자 확인 후 `gh pr review <번호> --approve --body "<무엇을 확인했는지 한 줄>"`.
- 작성자: `gh pr view <번호> --json reviewDecision`이 `APPROVED`이면, 사용자 확인 후 `gh pr merge <번호> --merge --delete-branch`. PR1 셋은 병합 순서가 상관없다.

관문 B: PR1 세 개가 모두 MERGED. 확인 후 `git switch main && git pull --ff-only` (은영은 PR2 브랜치에 그대로 둔다).

### 7단계 · 민영재 PR2 열기 (병합 금지)

```bash
git switch main && git pull --ff-only
gh issue create --title "docs: 민영재 자기소개 추가" --body "team/yeomin4242.md를 쓰고 README 팀원 표에 링크한다. 임시 메모를 지운다." --assignee @me
git switch -c feature/yeomin4242-team-intro
```

커밋을 꼭 세 개로 나눈다. 두 번째 커밋은 12단계에서 revert할 대상이다.

```bash
# 1) team/yeomin4242.md 작성 (사용자에게 물어서, team/README.md 형식)
git add team/yeomin4242.md && git commit -m "docs: 민영재 자기소개 추가"
# 2) 같은 파일 끝에 '## TMI' 섹션 한두 줄 추가 (사용자에게 물어서)
git commit -am "docs: 자기소개에 TMI 섹션 추가"
# 3) README.md 팀원 표의 민영재 줄 (작성 예정) → [소개](team/yeomin4242.md), 그리고 임시 메모 삭제
git rm docs/intro-notes.md
git commit -am "docs: README 팀원 표에 소개 링크 추가, 임시 메모 삭제"
git push -u origin HEAD
```

PR 생성, 리뷰어 `gpffh20`. PR 본문 "리뷰어에게" 칸에 "TMI 섹션은 12단계 revert 실습용이라 일부러 넣었어요"라고 적는다. **아직 병합하지 않는다.**

팀 채팅 한 줄: `[7단계] 영재 PR2 열었어요 (#번호). 병합은 10단계에서 해요.`

### 8단계 · 유민규 PR2 열기 (병합 금지)

```bash
git switch main && git pull --ff-only
gh issue create --title "docs: 유민규 자기소개 추가" --body "team/minkyu01.md를 쓰고 README 팀원 표에 링크한다." --assignee @me
git switch -c feature/minkyu01-team-intro
# team/minkyu01.md 작성 (사용자에게 물어서), README.md 팀원 표의 유민규 줄 → [소개](team/minkyu01.md)
echo '- 링크는 상대 경로로 쓴다' >> docs/intro-notes.md
git add team/minkyu01.md README.md docs/intro-notes.md
git commit -m "docs: 유민규 자기소개 추가, 작성 메모에 링크 규칙 추가"
git push -u origin HEAD
```

PR 생성, 리뷰어 `yeomin4242`. **아직 병합하지 않는다.**

관문 C: 영재 PR2와 민규 PR2가 둘 다 열려 있다.

팀 채팅 한 줄: `[8단계] 민규 PR2 열었어요 (#번호). 관문 C 통과, 은영 PR2 시작해도 돼요.`

### 9단계 · 신은영 PR2 → 병합 ④

```bash
git switch feature/gpffh20-team-intro         # 2단계부터 작업 중이던 브랜치
git add team/gpffh20.md README.md
git commit -m "docs: 신은영 자기소개 추가"
git add docs/troubleshooting-log.md
git commit -m "docs: stash 실습 기록 추가"
git push -u origin HEAD
```

PR 생성(`Closes` 2단계에서 만든 이슈), 리뷰어 `Minkyu01`. 유민규가 리뷰 → 은영이 반영 → 유민규 승인 → 은영이 병합.

팀 채팅 한 줄: `[9단계] 은영 PR2 병합했어요 (④). 영재 차례예요.`

### 10단계 · 민영재 충돌 #1 → 병합 ⑤

```bash
git switch feature/yeomin4242-team-intro
git fetch origin
git merge origin/main                          # CONFLICT (content): README.md — 출력 저장
sed -n '/^<<<<<<</,/^>>>>>>>/p' README.md      # 충돌 마커 부분 — 출력 저장
```

README 팀원 표에서 신은영 줄과 민영재 줄이 둘 다 링크가 되도록 마커를 정리한다. 정리 결과를 사용자에게 보여 주고 확인받는다.

```bash
git diff --check                               # 출력이 없어야 한다
git add README.md
git commit --no-edit
```

`docs/conflict-resolution.md`의 "충돌 기록 #1"을 채워 `git commit -am "docs: 충돌 기록 #1 작성"` → `git push`. 신은영이 리뷰(자기 줄이 살아 있는지) → 반영 → 승인 → 영재가 병합. TMI 섹션은 12단계 실습 대상이므로 리뷰에서 지우라고 하지 않는다.

팀 채팅 한 줄: `[10단계] 충돌 #1 풀고 병합했어요 (⑤). 민규 차례예요.`

### 11단계 · 유민규 충돌 #2 → 병합 ⑥

```bash
git switch feature/minkyu01-team-intro
git fetch origin
git merge origin/main                          # CONFLICT 두 개: README.md, (modify/delete) docs/intro-notes.md — 출력 저장
```

- `README.md`: 세 줄이 모두 링크가 되게 정리한다 (순서는 유민규, 신은영, 민영재 그대로).
- `docs/intro-notes.md`: 삭제를 받아들인다. 8단계에서 더한 줄 `- 링크는 상대 경로로 쓴다`는 `team/README.md`의 형식 목록 맨 아래로 옮긴다.

```bash
git rm docs/intro-notes.md
git add README.md team/README.md
git diff --cached --check
git commit --no-edit
```

"충돌 기록 #2"를 채워 커밋 → push. 민영재가 리뷰 → 반영 → 승인 → 민규가 병합.

이 충돌은 GitHub 웹의 "Resolve conflicts" 편집기로 풀 수 없다. 반드시 로컬에서 푼다.

팀 채팅 한 줄: `[11단계] 충돌 #2 풀고 병합했어요 (⑥). 영재 revert 차례예요.`

### 12단계 · 민영재 revert → 병합 ⑦

```bash
git switch main && git pull --ff-only
gh issue create --title "docs: 자기소개 TMI 섹션 되돌리기" --body "팀 소개 형식에 없는 TMI 섹션 커밋을 revert한다." --assignee @me
git switch -c feature/yeomin4242-revert-tmi
git log --oneline -- team/yeomin4242.md        # TMI 커밋 해시 확인 — 출력 저장
git revert --no-edit <TMI 커밋 해시>
git log --oneline -3                           # Revert "…" 커밋 — 출력 저장
```

"시나리오: revert" 절을 채워 커밋 → push → PR, 리뷰어 `Minkyu01` → 반영 → 승인 → 병합.

팀 채팅 한 줄: `[12단계] revert 병합했어요 (⑦). 민규 마무리 차례예요.`

### 13단계 · 유민규 SUBMISSION → 병합 ⑧

```bash
git switch main && git pull --ff-only
gh issue create --title "docs: SUBMISSION 제출물 인덱스 작성" --body "팀원별 이슈·PR·리뷰와 증빙을 한곳에 모은다." --assignee @me
git switch -c feature/minkyu01-submission
```

1. `SUBMISSION.md`를 10절 형식으로 쓴다. 번호와 링크는 gh로 모은다.
   ```bash
   gh issue list --state all --author <아이디> --json number,title,url
   gh pr list --state merged --author <아이디> --json number,title,url
   gh pr list --state all --search "reviewed-by:<아이디> -author:<아이디>" --json number,title,url
   ```
2. 두 기록 문서 맨 위 표의 "PR" 칸, troubleshooting-log의 "공통 환경" 줄을 채운다.
3. 사용자가 찍은 Branch Protection 설정 화면을 `docs/images/branch-protection.png`로 넣는다.
4. `git log --oneline --graph --all > docs/git-history.txt`
5. 커밋 → push → PR, 리뷰어 `gpffh20` → 반영 → 승인 → 병합.
6. 병합 후 `git switch main && git pull --ff-only && git log --oneline --graph --all`의 출력을 이 PR에 코멘트로 남긴다 (`gh pr comment <번호> --body-file <파일>`).

### 14단계 · 설명 연습 (모두)

사용자가 "설명 연습"이라고 하면 [docs/team-guide.md](docs/team-guide.md)의 설명 카드와 평가 질문에서 3개를 골라 하나씩 묻고, 답을 들은 뒤 빠진 부분만 짧게 알려 준다.

## 7. CONTRIBUTING 절별 내용

| 절 | 담당 | 꼭 들어갈 것 |
| --- | --- | --- |
| 1. 브랜치 전략 | 유민규 | `main`은 항상 깨지지 않는 상태(우리 기준: README·docs의 링크가 열리고 표가 깨지지 않음), 작업은 `feature/*`, main 보호 규칙 세 가지(PR 필수, 승인 1명, 관리자도 우회 불가), merge commit만 허용, 작업 단위 기준(이슈 1 = 브랜치 1 = PR 1, 목적 하나), **GitHub Flow를 고른 이유 3줄 이내** |
| 2. 브랜치 이름 | 유민규 | `feature/<GitHub 아이디 소문자>-<주제>`, 좋은 예와 나쁜 예, 되돌리기·긴급 수정도 같은 형식, 만드는 명령 |
| 3. 커밋 메시지 | 신은영 | `<type>: <무엇을>` 형식과 type 표, 금지 기준 두 가지(대상을 알 수 없는 단어만, 무엇을·왜가 없음), GitHub 웹 기본 메시지(`Update README.md`) 주의, 병합·revert 기본 메시지는 허용, push 전 확인 명령 `git log --oneline origin/main..HEAD` |
| 4. PR 규칙 | 신은영 | 제목 형식, 본문 필수 칸(`Closes #`, What, Why, How), 리뷰어 지정, 병합 조건(승인 1, 모든 코멘트에 답글, 충돌 없음, 차례표 순서), 작성자가 merge commit으로 병합, 1인당 최소 기준 |
| 5. 코드 리뷰 | 민영재 | PR마다 실질 코멘트 1개 이상(줄 코멘트, 말머리 네 가지), LGTM만으로 승인 금지, 반영 순서(코멘트 → 답글·반영 커밋 → Resolve → 승인), 리뷰 순번표 |
| 6. 충돌 대응 | 민영재 | 누가(충돌 난 PR 작성자), 어떻게(공유 → 자기 브랜치에서 `git merge origin/main` → 마커 정리 → `git diff --check` → 상대가 리뷰), 어디에 기록(`docs/conflict-resolution.md`), rebase 대신 merge를 쓰는 이유, 마커의 뜻 |
| 7. 히스토리 규칙 | 유민규 | 공유 브랜치 히스토리 재작성 금지, 합의 없는 강제 push 금지, push 전에만 amend·reset, push 후에는 revert(feature 브랜치에서 PR로) |

각 절은 담당자가 자기 PR에서 그 절만 고친다. 절 제목과 다른 사람 절은 건드리지 않는다.

## 8. 기록 쓰는 법

`docs/troubleshooting-log.md`와 `docs/conflict-resolution.md`에 이미 있는 절 구조(참여자, 상황, 명령, 결과, 이유)를 그대로 채운다.

- 명령과 출력은 코드 블록에 실제 터미널 내용을 붙인다.
- "결과"에는 무엇이 바뀌었는지와 주의할 점(특히 원격·팀원에게 미치는 영향)을 쓴다.
- "왜 이 방법을 선택했는가"에는 다른 방법 대신 이걸 쓴 이유를 한두 줄로 쓴다 (예: reset 대신 revert).
- `(작성 예정)`은 모두 지운다.

## 9. 리뷰하는 법

```bash
N=<PR 번호>
TMP=$(mktemp -d)                               # 임시 파일은 저장소 밖에
gh pr diff $N                                  # 바뀐 줄 보기
SHA=$(gh pr view $N --json headRefOid -q .headRefOid)
```

1. diff에서 `+`로 시작하는 줄 중 근거로 삼을 줄을 고르고, 그 파일에서 몇 번째 줄인지 확인한다 (`grep -n`).
2. 코멘트 초안 1~2개(말머리 포함)를 사용자에게 보여 주고 확인받는다.
3. 게시:
   ```bash
   cat > "$TMP/review.json" <<EOF
   {"commit_id":"$SHA","event":"COMMENT","body":"리뷰 남겨요.","comments":[{"path":"<파일>","line":<줄>,"side":"RIGHT","body":"[질문] <내용>"}]}
   EOF
   gh api repos/{owner}/{repo}/pulls/$N/reviews --method POST --input "$TMP/review.json"
   ```
4. 작성자로서 답글 달기:
   ```bash
   gh api repos/{owner}/{repo}/pulls/$N/comments --jq '.[] | {id, path, line, user: .user.login, body}'
   gh api repos/{owner}/{repo}/pulls/$N/comments/<코멘트 id>/replies --method POST -f body="반영했어요: <커밋 해시>"
   ```

## 10. SUBMISSION.md 형식

```markdown
# Submission Index

## Team
- 팀명: <팀명>
- 저장소: https://github.com/Minkyu01/B2-2_git-collab-workflow
- 구성: 옵션 B (개인 공개 저장소 + Collaborator)
- 간단한 결과물: (B) 팀 소개 — README 팀원 표

## Member Issues · PRs · Reviews
### <이름> (@<아이디>)
- Issue: #, #
- PR (병합): #, #
- 리뷰한 PR: #, #
- 리뷰 반영: PR # — <코멘트 링크> → <반영 커밋 링크>
- 팀 소개 커밋: <커밋 링크>
- 트러블슈팅 기록: <시나리오> (<역할>)

## 팀원별 최소 기준 점검
(PR 병합 2, 리뷰 2, 리뷰 반영 1, 기록 참여 1, 소개 커밋 1 × 3명 표)

## Key Docs
- docs/CONTRIBUTING.md (GitHub Flow를 고른 이유는 1절), docs/conflict-resolution.md, docs/troubleshooting-log.md

## Evidence
- docs/git-history.txt, docs/images/branch-protection.png
- main에 직접 push가 없다는 근거: `git log --first-parent --oneline origin/main`
```

## 11. 막혔을 때

- `GH006: Protected branch update failed`: main에 바로 push했다. 지금 브랜치를 확인하고 feature 브랜치에서 다시 한다.
- 이미 push한 커밋 메시지가 규칙 위반: 고치지 않는다(강제 push 금지). 다음 커밋부터 지키고 PR 본문에 적는다.
- 충돌이 계획과 다르게 났거나 나지 않았다: 진행을 멈추고 사용자에게 알린다. 사용자가 팀 채팅에 공유한다.
- 차례가 아닌데 병합하려 한다: 하지 않는다. 기다릴 단계를 알려 준다.
- gh가 없거나 로그인이 안 된다: GitHub 웹에서 할 순서를 사용자에게 안내한다.
