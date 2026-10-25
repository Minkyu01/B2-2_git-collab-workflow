# Conflict Resolution Log

팀에서 일부러 낸 충돌과 해결 과정을 남긴다. 대응 규칙은 [CONTRIBUTING.md 6절](CONTRIBUTING.md)을 따른다.

| # | 종류 | 비자명 기준 | 해결 | 상대 | PR |
| --- | --- | --- | --- | --- | --- |
| 1 | 같은 파일의 인접 라인 수정 (`README.md` 팀원 표) | (1) 같은 hunk | 민영재 | 신은영 | [#10](https://github.com/Minkyu01/B2-2_git-collab-workflow/pull/10) |
| 2 | 삭제 + 수정 (`docs/intro-notes.md`) | (2) 삭제 vs 수정 | 유민규 | 민영재 | [#11](https://github.com/Minkyu01/B2-2_git-collab-workflow/pull/11) |

---

## 충돌 기록 #1

### 참여자
- 작성자: 민영재 (@yeomin4242)
- 상대: 신은영 (@gpffh20)

### 상황(What happened)
- PR #10에서 README 팀원 표의 민영재 행에 소개 링크를 추가했다. 은영 PR #12가 먼저 병합되며 신은영 행에도 소개 링크가 추가되어, 인접한 두 행이 서로 다른 내용으로 바뀌었다.
- PR #12를 받은 뒤 두 행이 같은 충돌 구간에 들어가 Git이 자동 병합하지 못했다. 양쪽 링크를 모두 남겨야 했다.

### 충돌 내용(Conflict markers)
<pre><code>&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD
| 신은영 | @gpffh20 | (작성 예정) |
| 민영재 | @yeomin4242 | [소개](team/yeomin4242.md) |
&#61;&#61;&#61;&#61;&#61;&#61;&#61;
| 신은영 | @gpffh20 | [소개](team/gpffh20.md) |
| 민영재 | @yeomin4242 | (작성 예정) |
&gt;&gt;&gt;&gt;&gt;&gt;&gt; origin/main</code></pre>

### 해결 과정(How)
```bash
$ git fetch origin && git merge origin/main
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.

$ sed -n '/^<<<<<<< /,/^>>>>>>> /p' README.md
```
<pre><code>&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD
| 신은영 | @gpffh20 | (작성 예정) |
| 민영재 | @yeomin4242 | [소개](team/yeomin4242.md) |
&#61;&#61;&#61;&#61;&#61;&#61;&#61;
| 신은영 | @gpffh20 | [소개](team/gpffh20.md) |
| 민영재 | @yeomin4242 | (작성 예정) |
&gt;&gt;&gt;&gt;&gt;&gt;&gt; origin/main</code></pre>

```bash
$ git diff --check
$ git add README.md
$ git diff --cached --check
```

`HEAD` 쪽의 민영재 링크와 `origin/main` 쪽의 신은영 링크를 모두 남기고 마커를 지웠다. `git diff --check`와 `git diff --cached --check`는 출력을 내지 않았다.

```bash
$ git commit --no-edit
[feature/yeomin4242-team-intro bec8bfc] Merge remote-tracking branch 'origin/main' into feature/yeomin4242-team-intro
```

### 결과(Outcome)
- README 팀원 표에서 신은영과 민영재의 소개 링크를 모두 보존했고, merge commit `bec8bfc`로 origin/main을 feature 브랜치에 합쳤다. 이 기록은 별도 문서 커밋으로 추가한다.

### 배운 점(Learnings)
- README처럼 인접한 행을 여러 PR이 바꾸면 충돌이 날 수 있다. 양쪽 PR의 의도를 확인하고 두 링크를 함께 남겨야 한다.
- 커밋하기 전에 충돌 상대에게 정리 결과를 공유하고, 상대가 PR 리뷰에서 자기 줄이 살아 있는지 확인한다.

---

## 충돌 기록 #2

### 참여자
- 작성자: 유민규 (@Minkyu01)
- 상대: 민영재 (@yeomin4242)

### 상황(What happened)
- 브랜치 `feature/minkyu01-team-intro`(PR #11)에서 `git merge origin/main`을 했다. 이 브랜치는 PR1이 병합된 `main`에서 갈라졌고, 그 사이 은영의 PR #12(④)와 영재의 PR #10(⑤)이 병합됐다.
- 충돌이 두 곳에서 났다.
  1. `README.md`: 내 PR은 내 줄만 소개 링크로 바꿨고, main에는 은영과 영재 줄이 링크로 바뀌어 있다. 인접한 세 줄이 서로 다르게 바뀌었다.
  2. `docs/intro-notes.md`: 영재 PR #10이 이 임시 메모를 지웠고(`git rm`), 내 PR은 같은 파일 끝에 한 줄을 더했다. 한쪽은 삭제, 한쪽은 수정이라 `modify/delete` 충돌이다 (비자명 기준 (2)).
- 이 메모는 원래 "세 명의 소개가 모두 병합되면 지운다"고 적힌 임시 파일이다.

### 충돌 내용(Conflict markers)
`git merge`의 출력과 충돌한 파일 목록 (실제 출력):

```bash
$ git fetch origin
$ git merge origin/main
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
CONFLICT (modify/delete): docs/intro-notes.md deleted in origin/main and modified in HEAD.  Version HEAD of docs/intro-notes.md left in tree.
Automatic merge failed; fix conflicts and then commit the result.
$ git status --short
UU README.md
M  docs/conflict-resolution.md
UD docs/intro-notes.md
M  docs/troubleshooting-log.md
A  team/gpffh20.md
A  team/yeomin4242.md
```

`README.md`의 충돌 구간 (실제 내용. 마커 줄이 `git diff --check`에 걸리지 않게 HTML 코드로 적었다):

<pre><code>&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD
| 유민규 | @Minkyu01 | [소개](team/minkyu01.md) |
| 신은영 | @gpffh20 | (작성 예정) |
| 민영재 | @yeomin4242 | (작성 예정) |
&#61;&#61;&#61;&#61;&#61;&#61;&#61;
| 유민규 | @Minkyu01 | (작성 예정) |
| 신은영 | @gpffh20 | [소개](team/gpffh20.md) |
| 민영재 | @yeomin4242 | [소개](team/yeomin4242.md) |
&gt;&gt;&gt;&gt;&gt;&gt;&gt; origin/main</code></pre>

`docs/intro-notes.md`는 파일 안에 마커가 없다. 상태가 `UD`(내 쪽 수정, 상대 쪽 삭제)로만 나온다.

### 해결 과정(How)
- `README.md`: 양쪽이 서로 다른 줄을 링크로 바꾼 것이라 둘 다 살린다 (keep both). 세 줄이 모두 소개 링크가 되게 했고, 순서는 유민규, 신은영, 민영재를 그대로 둔다.
- `docs/intro-notes.md`: 삭제를 받아들인다 (choose theirs). 이유는 세 가지다.
  1. 세 명의 소개가 모두 병합되면 지우기로 한 임시 메모다.
  2. 영재 PR #10이 이미 지웠다.
  3. 내가 더한 규칙 한 줄(`링크는 상대 경로로 쓴다`)은 앞으로도 쓸모가 있어서, 버리지 않고 `team/README.md`의 형식 목록 맨 아래로 옮긴다.
- 충돌 상대인 영재는 이 PR의 리뷰어로, 자기 PR의 삭제가 그대로 반영됐는지 확인한다.
- 실제로 한 명령과 출력:

```bash
$ git fetch origin
$ git merge origin/main
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
CONFLICT (modify/delete): docs/intro-notes.md deleted in origin/main and modified in HEAD.  Version HEAD of docs/intro-notes.md left in tree.
Automatic merge failed; fix conflicts and then commit the result.

$ git status --short
UU README.md
M  docs/conflict-resolution.md
UD docs/intro-notes.md
M  docs/troubleshooting-log.md
A  team/gpffh20.md
A  team/yeomin4242.md

# README.md의 마커를 지우고 세 줄을 링크로 정리, team/README.md에 규칙 한 줄 추가 (편집기로 직접)
$ git diff --check   # README 정리 직후, add 전
(출력 없음)

$ git rm docs/intro-notes.md
rm 'docs/intro-notes.md'
$ git add README.md team/README.md
$ git diff --cached --check
(출력 없음)
$ git status --short
M  README.md
M  docs/conflict-resolution.md
D  docs/intro-notes.md
M  docs/troubleshooting-log.md
M  team/README.md
A  team/gpffh20.md
A  team/yeomin4242.md

$ git commit -q --no-edit   # -q: 출력 생략
$ git log --oneline -3
7bce65a Merge remote-tracking branch 'origin/main' into feature/minkyu01-team-intro
481c1f4 Merge pull request #10 from Minkyu01/feature/yeomin4242-team-intro
c4c14f6 fix: 리뷰 반영 - 충돌 기록 재현 순서 보완
```

### 결과(Outcome)
- 병합 커밋 `7bce65a`로 `origin/main`을 feature 브랜치에 합쳤다. README 팀원 표 세 줄이 모두 소개 링크가 됐고, 임시 메모는 삭제(영재 PR의 의도 유지), 내 규칙 한 줄은 `team/README.md`로 옮겨졌다.
- 확인: `git diff --check`와 `git diff --cached --check`의 출력이 없고, 충돌 마커가 남지 않았다.
- 해결한 내용은 `git show --remerge-diff 7bce65a`로 다시 볼 수 있다. README는 `<<<<<<<` 구간이 세 줄 링크로 바뀌고, `docs/intro-notes.md`는 삭제되고, `team/README.md`에 한 줄이 늘어난다.
- 관련 PR: #11 (이슈 #9)

### 배운 점(Learnings)
- 한 PR이 지우는 파일을 다른 PR이 고치면 `modify/delete` 충돌이 난다. 임시 파일은 "누가 언제 지우는지"를 먼저 정해 두고, 지우기 전에 남길 내용은 다른 곳으로 옮긴다.
- 삭제가 낀 충돌은 파일 안에 마커가 없어서 `git diff --check`만으로는 놓칠 수 있다. `git status --short`에서 `UD`가 사라졌는지와 add/rm 뒤의 `git diff --cached --check`를 같이 본다. (PR #6 리뷰에서 제안한 내용을 실제로 써 봤다.)
- GitHub 웹의 충돌 해결 편집기는 이런 삭제 충돌을 풀지 못해서 로컬에서 풀었다.
