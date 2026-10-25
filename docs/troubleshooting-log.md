# Troubleshooting Log

Git으로 실수를 바로잡은 과정을 남긴다. 누가 하더라도 같은 순서로 다시 해 볼 수 있게, 실제로 친 명령과 실행 전·후 출력을 그대로 붙인다.

| 시나리오 | 수행·기록 | 검토 | 관련 PR |
| --- | --- | --- | --- |
| 1. `git commit --amend` | 유민규 | 신은영 | |
| 2. `git reset --soft HEAD~1` | 신은영 | 민영재 | |
| 3. `git revert` | 민영재 | 유민규 | |
| 4. `git stash` / `git stash pop` | 신은영 | 유민규 | |

검토 = 그 PR의 리뷰어가 기록대로 다시 해 보고 확인한다.

공통 환경: (작성 예정 — `git --version`, OS)

---

## 시나리오: amend
### 참여자
- 유민규 (수행·기록), 신은영 (검토)

### 상황
- (작성 예정)

### 시도한 명령/절차
```bash
(작성 예정)
```

### 결과
- (작성 예정)

### 왜 이 방법을 선택했는가(Why)
- (작성 예정)

---

## 시나리오: reset
### 참여자
- 신은영 (수행·기록), 민영재 (검토)

### 상황
- (작성 예정)

### 시도한 명령/절차
```bash
(작성 예정)
```

### 결과
- (작성 예정)

### 왜 이 방법을 선택했는가(Why)
- (작성 예정)

---

## 시나리오: revert
### 참여자
- 민영재 (수행·기록), 유민규 (검토)

### 상황
- (작성 예정)

### 시도한 명령/절차
```bash
(작성 예정)
```

### 결과
- (작성 예정)

### 왜 이 방법을 선택했는가(Why)
- (작성 예정)

---

## 시나리오: stash
### 참여자
- 신은영 (수행·기록), 유민규 (검토)

### 상황
- `feature/gpffh20-team-intro`(PR2)에서 자기소개 파일 `team/gpffh20.md`(새 파일)와 `README.md` 팀원 표 링크를 커밋하지 않은 채 작성하던 중, PR1(#2)에 민영재의 리뷰 코멘트가 달렸다.
- 덜 쓴 자기소개를 커밋하지 않고 PR1 브랜치로 옮겨 가서 리뷰를 반영해야 했다.

### 시도한 명령/절차
```bash
# 전: PR2 작업이 커밋되지 않은 채 남아 있다
$ git status --short
 M README.md
?? team/gpffh20.md

# -u: 추적하지 않는 새 파일(team/gpffh20.md)도 같이 보관
$ git stash push -u -m "PR2 자기소개 작성 중"
Saved working directory and index state On feature/gpffh20-team-intro: PR2 자기소개 작성 중
$ git stash list
stash@{0}: On feature/gpffh20-team-intro: PR2 자기소개 작성 중
$ git status --short
$ git switch feature/gpffh20-contributing-commit-pr
Switched to branch 'feature/gpffh20-contributing-commit-pr'
Your branch is up to date with 'origin/feature/gpffh20-contributing-commit-pr'.

# PR1에서 리뷰 반영
$ git commit -am "fix: 리뷰 반영 - PR 본문 규칙에 실습 기록 출력 첨부 기준 추가"
[feature/gpffh20-contributing-commit-pr 5cac2e4] fix: 리뷰 반영 - PR 본문 규칙에 실습 기록 출력 첨부 기준 추가
 1 file changed, 1 insertion(+)
$ git push
   abf8e6d..5cac2e4  feature/gpffh20-contributing-commit-pr -> feature/gpffh20-contributing-commit-pr

# PR2로 돌아와서 보관한 작업을 꺼낸다
$ git switch feature/gpffh20-team-intro
Switched to branch 'feature/gpffh20-team-intro'
$ git stash pop
On branch feature/gpffh20-team-intro
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	team/gpffh20.md

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (4cba7b159a9a08b5e0b0dd20cc083c781870e8f8)

# 후: 작업이 그대로 돌아왔고 stash는 비었다
$ git status --short
 M README.md
?? team/gpffh20.md
$ git stash list
```

### 결과
- stash 직후 작업 트리가 깨끗해져서 PR1 브랜치로 바로 옮겨 갈 수 있었다. PR1에 반영 커밋 `5cac2e4`를 push한 뒤, PR2로 돌아와 pop하자 `README.md` 수정과 새 파일 `team/gpffh20.md`가 모두 돌아왔다.
- `pop`은 꺼낸 뒤 stash 목록에서 지운다(`Dropped refs/stash@{0}`). 꺼낼 때 충돌이 나면 stash가 지워지지 않고 남으니, 충돌을 정리한 뒤 `git stash drop`으로 직접 지운다.
- 주의: `-u` 없이 stash하면 추적하지 않는 새 파일은 보관되지 않고 작업 트리에 남는다. 그 상태로 브랜치를 옮기면 새 파일이 다른 브랜치 작업에 섞일 수 있다.
- stash는 내 컴퓨터에만 있고 push되지 않는다. 원격과 팀원에게는 영향이 없다.

### 왜 이 방법을 선택했는가(Why)
- 덜 쓴 자기소개를 `wip` 같은 커밋으로 남기면 커밋 규칙(CONTRIBUTING 3절)을 어기고 PR2 히스토리도 지저분해진다. stash는 커밋 없이 작업을 잠깐 치워 둘 수 있다.
- 브랜치마다 작업 폴더를 따로 두는 `git worktree`도 있지만, 잠깐 다른 브랜치를 고치고 돌아오는 정도에는 stash가 가장 단순하다.
