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
- 2026-10-25, `feature/minkyu01-contributing-flow` 브랜치에서 CONTRIBUTING 1·2·7절을 쓰고 커밋했다. 커밋 메시지를 일부러 규칙 위반인 `docs: update`(무엇을 바꿨는지 없음)로 남긴 상태를 만들고, push하기 전에 `--amend`로 고치는 실습이다.
- 아직 push 전이라 아무도 이 커밋을 받지 않았다.

### 시도한 명령/절차
```bash
$ git add docs/CONTRIBUTING.md
$ git commit -m "docs: update"
[feature/minkyu01-contributing-flow aaf7144] docs: update
 1 file changed, 42 insertions(+), 3 deletions(-)
$ git log --oneline -1
aaf7144 docs: update
$ git commit --amend -m "docs: CONTRIBUTING 1·2·7절 작성"
[feature/minkyu01-contributing-flow bb633bb] docs: CONTRIBUTING 1·2·7절 작성
 Date: Sun Oct 25 15:07:21 2026 +0900
 1 file changed, 42 insertions(+), 3 deletions(-)
$ git log --oneline -1
bb633bb docs: CONTRIBUTING 1·2·7절 작성
```

### 결과
- 메시지가 `docs: update` → `docs: CONTRIBUTING 1·2·7절 작성`으로 고쳐졌다. 커밋 해시도 `aaf7144` → `bb633bb`로 바뀌었다 (amend는 커밋을 새로 만들어 바꿔치기한다).
- `git log --oneline origin/main..HEAD`에 `docs: update`는 남지 않는다.
- 고치기 전 커밋 `aaf7144`는 브랜치에서는 사라졌지만 `git reflog`에서 찾을 수 있다 (실제 출력, 번호는 실행 시점마다 다르다):
  ```bash
  $ git reflog | grep -E 'aaf7144|bb633bb'
  bb633bb HEAD@{1}: commit (amend): docs: CONTRIBUTING 1·2·7절 작성
  aaf7144 HEAD@{2}: commit: docs: update
  ```
- 다른 컴퓨터에서 다시 하면 해시는 달라진다 (작성자·시각이 해시에 들어가므로). 같은 것은 "amend 뒤 해시가 바뀐다"는 점이다.- 위 출력은 실제 터미널 내용이다. 도구가 붙이는 `Co-Authored-By` 줄은 생략했다.
- 주의할 점: 이미 push한 커밋에 amend하면 원격 히스토리와 갈라져 강제 push가 필요하다. 그래서 push 전에만 쓴다. 이 브랜치는 이 실습 동안 push하지 않았다.

### 왜 이 방법을 선택했는가(Why)
- 메시지만 틀렸고 push 전이라 아무도 받지 않은 커밋이었다. `fix:` 커밋을 더 쌓으면 잘못된 `docs: update`가 PR 히스토리에 그대로 남지만, amend하면 남지 않는다.
- 이미 push한 뒤였다면 amend하지 않고, 다음 커밋부터 규칙을 지키고 PR 본문에 적는다 (CONTRIBUTING 7절).

---

## 시나리오: reset
### 참여자
- 신은영 (수행·기록), 민영재 (검토)

### 상황
- `feature/gpffh20-contributing-commit-pr` 브랜치에서 CONTRIBUTING 3·4절을 쓰고 `git add .`로 커밋했더니, 저장소에 올리면 안 되는 개인 메모 `todo.txt`까지 커밋에 들어갔다.
- 아직 push하기 전이라 원격과 팀원에게는 이 커밋이 없다.

### 시도한 명령/절차
```bash
$ git status --short
 M docs/CONTRIBUTING.md
$ echo "개인 메모" > todo.txt
$ git add .
$ git commit -m "docs: CONTRIBUTING 3·4절 작성"
[feature/gpffh20-contributing-commit-pr b8b7a79] docs: CONTRIBUTING 3·4절 작성
 2 files changed, 42 insertions(+), 2 deletions(-)
 create mode 100644 todo.txt

# 전: 커밋에 todo.txt가 들어 있다
$ git show --name-only --format= HEAD
docs/CONTRIBUTING.md
todo.txt

# 마지막 커밋만 취소하고, 바뀐 내용은 스테이징에 남긴다
$ git reset --soft HEAD~1
$ git status --short
M  docs/CONTRIBUTING.md
A  todo.txt

# todo.txt만 스테이징에서 뺀다 (파일은 그대로 남는다)
$ git restore --staged todo.txt
$ git status --short
M  docs/CONTRIBUTING.md
?? todo.txt

$ git commit -m "docs: CONTRIBUTING 3·4절 작성"
[feature/gpffh20-contributing-commit-pr 489502f] docs: CONTRIBUTING 3·4절 작성
 1 file changed, 41 insertions(+), 2 deletions(-)

# 후: CONTRIBUTING.md만 들어 있다
$ git show --name-only --format= HEAD
docs/CONTRIBUTING.md

$ rm todo.txt
$ git status --short
$ git log --oneline -2
489502f docs: CONTRIBUTING 3·4절 작성
277534c chore: 저장소 기본 구조, 협업 템플릿, 팀 작업 안내 추가
```

### 결과
- 잘못된 커밋 `b8b7a79`는 브랜치에서 사라지고, `docs/CONTRIBUTING.md`만 담은 커밋 `489502f`가 그 자리에 생겼다. CONTRIBUTING 수정 내용은 하나도 잃지 않았다.
- `--soft`라서 커밋만 취소되고 작업 내용과 스테이징은 남는다. `--hard`를 쓰면 작업 내용까지 지워지니 쓰지 않는다.
- 주의: reset은 커밋 히스토리를 바꾼다. push 전이라 원격과 팀원에게 영향이 없었다. 이미 push한 커밋에 reset을 쓰면 강제 push가 필요해지고 팀원의 히스토리와 어긋나므로, push 후에는 revert를 쓴다 (CONTRIBUTING 7절).
- 처음부터 `git add .` 대신 파일을 지정해서 add했다면 생기지 않았을 실수다.

### 왜 이 방법을 선택했는가(Why)
- 아직 push하지 않은 커밋이라 히스토리를 고쳐도 아무에게도 영향이 없다. revert는 잘못된 커밋과 되돌리는 커밋이 둘 다 남아 `todo.txt`가 히스토리에 계속 남지만, `reset --soft`는 커밋 자체를 없애고 내용은 보존한다.
- `--amend`로도 고칠 수 있지만, 커밋 단위로 다시 고르는(어떤 파일을 넣을지) 흐름을 명확히 보여 주려고 reset을 썼다.

---

## 시나리오: revert
### 참여자
- 민영재 (수행·기록), 유민규 (검토)

### 상황
- PR #10에서 팀 소개에 넣어 둔 TMI를 12단계 실습 대상으로 삼았다. PR #10과 유민규 PR #11이 이미 main에 병합되어 TMI 추가 커밋 `b54138f`도 공유 히스토리에 들어 있다.
- main의 기존 커밋을 지우거나 바꾸지 않고, TMI 추가만 취소하는 새 커밋을 만든다.

### 시도한 명령/절차
```bash
$ git log --oneline -- team/yeomin4242.md
b54138f docs: 자기소개에 TMI 섹션 추가
9518141 docs: 민영재 자기소개 추가

$ git revert --no-edit b54138f
[feature/yeomin4242-revert-tmi a983b0c] Revert "docs: 자기소개에 TMI 섹션 추가"
 Date: Sun Oct 25 16:08:19 2026 +0900
 1 file changed, 4 deletions(-)

$ git log --oneline -3
a983b0c Revert "docs: 자기소개에 TMI 섹션 추가"
5b50590 Merge pull request #11 from Minkyu01/feature/minkyu01-team-intro
ae8a944 docs: 충돌 기록 #2 작성

$ git log --oneline | grep b54138f
b54138f docs: 자기소개에 TMI 섹션 추가
```

### 결과
- `team/yeomin4242.md`에서 TMI 섹션 네 줄이 제거됐고, revert 커밋 `a983b0c`가 새로 생겼다. 원래 추가 커밋 `b54138f`는 히스토리에 남아 있다.
- 주의: revert는 이미 공유된 커밋을 지우지 않는다. 팀원은 평소처럼 `git pull`만 하면 새 revert 커밋을 받아오며, 강제 push가 필요 없다.

### 왜 이 방법을 선택했는가(Why)
- TMI를 추가한 커밋은 이미 main에 병합되어 공유됐다. reset으로 그 커밋을 없애면 공유 히스토리를 다시 쓰게 되지만, revert는 반대 변경을 새 커밋으로 추가해 이력을 보존한다.

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
# 다른 브랜치로 옮겨 가서 급한 작업을 한다. 이 브랜치는 PR #2 병합 때 지워졌으니,
# 다시 해 볼 때는 아무 다른 브랜치(예: main)로 옮겨 가도 같다.
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
