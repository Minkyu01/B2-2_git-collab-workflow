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
- (작성 예정)

### 시도한 명령/절차
```bash
(작성 예정)
```

### 결과
- (작성 예정)

### 왜 이 방법을 선택했는가(Why)
- (작성 예정)
