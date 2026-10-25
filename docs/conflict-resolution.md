# Conflict Resolution Log

팀에서 일부러 낸 충돌과 해결 과정을 남긴다. 대응 규칙은 [CONTRIBUTING.md 6절](CONTRIBUTING.md)을 따른다.

| # | 종류 | 비자명 기준 | 해결 | 상대 | PR |
| --- | --- | --- | --- | --- | --- |
| 1 | 같은 파일의 인접 라인 수정 (`README.md` 팀원 표) | (1) 같은 hunk | 민영재 | 신은영 | |
| 2 | 삭제 + 수정 (`docs/intro-notes.md`) | (2) 삭제 vs 수정 | 유민규 | 민영재 | |

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
```

`HEAD` 쪽의 민영재 링크와 `origin/main` 쪽의 신은영 링크를 모두 남기고 마커를 지웠다. `git diff --check`는 출력을 내지 않았다.

```bash
$ git commit --no-edit
[feature/yeomin4242-team-intro bec8bfc] Merge remote-tracking branch 'origin/main' into feature/yeomin4242-team-intro
```

### 결과(Outcome)
- README 팀원 표에서 신은영과 민영재의 소개 링크를 모두 보존했고, merge commit `bec8bfc`로 origin/main을 feature 브랜치에 합쳤다. 이 기록은 별도 문서 커밋으로 추가한다.

### 배운 점(Learnings)
- README처럼 인접한 행을 여러 PR이 바꾸면 충돌이 날 수 있다. 양쪽 PR의 의도를 확인하고 두 링크를 함께 남겨야 한다.
- 충돌 해결 결과를 커밋하기 전에 사용자에게 보여 주고 확인받는다.

---

## 충돌 기록 #2

### 참여자
- 작성자: 유민규 (@Minkyu01)
- 상대: 민영재 (@yeomin4242)

### 상황(What happened)
- (작성 예정)

### 충돌 내용(Conflict markers)
```txt
(작성 예정)
```

### 해결 과정(How)
- (작성 예정)

### 결과(Outcome)
- (작성 예정)

### 배운 점(Learnings)
- (작성 예정)
