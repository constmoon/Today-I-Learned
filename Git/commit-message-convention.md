# Commit Message Convention
커밋 메세지를 작성할 때는 규칙을 정하고 일관성 있게 작성해야 한다. 여러가지 커밋 스타일이 있지만, [Udacity](https://udacity.github.io/git-styleguide/)에서 소개하는 가이드를 참고하였다.

## 1. Commit Message Structure
커밋 메시지는 제목, 본문(선택), 꼬리말(선택)로 구성된다. 레이아웃은 다음과 같다.
```
type: subject

body

footer
```

## 2. Commit Type
커밋의 종류는 제목에 포함되어야한다.
- feat : 새로운 기능 추가
- fix : 버그 수정
- docs : 문서 수정
- style : 코드 포맷팅, 세미콜론 누락, 코드 변경이 없는 경우
- refactor : 코드 리펙토링
- test : 테스트 코드, 리펙토링 테스트 코드 추가
- chore : 빌드 업무 수정, 패키지 매니저 수정

## 3. Subject
- 제목은 최대 50자를 넘기지 않도록 한다.
- 영어로 작성시 대문자로 시작하고 마침표를 붙이지 않는다.
- 과거시제를 사용하지 않고 명령어로 작성한다. 내가 어떻게 했는지가 아닌, 이 커밋이 프로젝트에서 무슨 역할을 하는지가 중요하다. (`어떻게` 보다는 `무엇을`, `왜`에 맞춰 작성)
- "changed" -> "change"
- "Added" -> "add"

## 4. Body
- 선택사항이기 때문에 모든 커밋마다 본문을 작성할 필요는 없다.
- 부연설명이 필요하거나 커밋의 이유를 설명해야할 때 작성한다.
- 본문을 작성할 때는 제목과 구분하기 위해 한 칸 정도 띄어서 작성한다.

## 5. Footer
- 해당 커밋이 참조한 이슈 ID를 적는 등 선택사항이다.

## 6. Example
```
feat: Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequenses of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, preceded
by a single space, with blank lines in between, but conventions
vary here

If you use an issue tracker, put references to them at the bottom,
like this:

Resolves: #123
See also: #456, #789
```
- [타입] [컴포넌트]: "무엇을 추가했는지"
- `docs(backend): Update readme`, `feat(settings): add read-only display of user email`
