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
- (작성 예정)

### 시도한 명령/절차
```bash
(작성 예정)
```

### 결과
- (작성 예정)

### 왜 이 방법을 선택했는가(Why)
- (작성 예정)
