# 변경사항을 부분적으로 추가하기
얼마 전에 공용 리파지토리에서 작업을 하다가, 변경된 부분에 대해 단계적으로 커밋을 해야할 일이 있었다. 이때 배운 옵션을 적어보려 한다.


## git add .
git을 처음 배울 때 제일 많이 사용한 옵션이다. 변화가 일어난 파일과 추가로 생성된 파일 모두 add 하는 옵션이다. 파일명을 지정할 필요가 없어 편리한 옵션이지만, 어떤 내용이 커밋에 포함되는지 알 수가 없다. 나중에 커밋된 걸 보고나서야 "아 맞다..." 라며 의도치 않은 수정사항이 있었다는 걸 깨닫고 리셋을 하는 뻘짓을 종종 했었다. 이를 방지하기 위해 add 단계에서 어떤 내용이 추가되는지를 확인하며 커밋을 진행하는 걸 권장한다.


## git add -p
일반적으로 `git status`로 파일 목록을 확인하고 원하는 파일만 `git add FIELNAME`로 추가를 하는데, 이 두 과정을 합친 게 `git add -p`이다. git add -p를 하면 파일에서 수정된 부분을 단위별로 나누어서 추가할지 여부를 물어본다. 
```
Stage this hunk [y,n,q,a,d,/,e,?]?
```
하나의 변경사항 단위를 hunk라고 한다. 이러한 hunk 단위로 변경사항을 추가할지 말지를 정할 수 있다. 각 옵션에 대한 설명은 아래와 같다.
```
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk nor any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk nor any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
```
y를 누르면 해당 hunk를 스테이징에 추가하고, n을 누르면 추가하지 않은채 다음 hunk로 이동한다. q를 누르면 add 과정을 종료한다.
