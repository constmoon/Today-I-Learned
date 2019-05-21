# Fork한 로컬 저장소를 원격 저장소(Original Repository)와 동기화하는 방법
내 계정에 Fork한 저장소(Repository)에서 작업하고 있는데, 원격 저장소(Original Repository)에 업데이트(새로운 commit)가 생겼을 때 변경 내역을 추가하는 방법을 알아본다.

## 1. `git remote -v`로 현재 로컬 저장소의 remote 정보를 확인한다.
```
❯ git remote -v
origin    https://github.com/constmoon/isnamyang (fetch)
origin    https://github.com/constmoon/isnamyang (push)
```

## 2. 원격 저장소에 'upstream'이란 이름을 붙이고 원격 저장소 내용을 추가한다. 
이름은 상관 없지만, Github에서는 upstream/downstream 개념을 사용하면서 원격 저장소를 upstream이라고 부른다. 일반적으로 많이 사용되는듯
```
❯ git remote add upstream https://github.com/nullfull/isnamyang
```

## 3. 다시 remote 정보를 확인하면, 'upstream' 이름으로 원격 저장소가 추가되었음을 볼 수 있다.
```
❯ git remote -v
origin    https://github.com/constmoon/isnamyang (fetch)
origin    https://github.com/constmoon/isnamyang (push)
upstream    https://github.com/nullfull/isnamyang (fetch)
upstream    https://github.com/nullfull/isnamyang (push)
```

## 4. 추가한 upstream 저장소를 `fetch`한다. Fetch는 pull과 다르게 변경 내역을 자동으로 merge하지 않는다.
```
❯ git fetch upstream
remote: Enumerating objects: 21, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 9 (delta 3), reused 7 (delta 2), pack-reused 0
Unpacking objects: 100% (9/9), done.
From https://github.com/nullfull/isnamyang
* [new branch]      master     -> upstream/master
```
upstream 저장소의 master 브랜치를 정상적으로 가져왔다.

## 5. 내 계정의 로컬 저장소의 master를 checkout 한다. 
현재 작업 중인 로컬 저장소의 경우 브랜치가 master 밖에 없어서 생략해도 무방.
```
❯ git checkout master
Already on 'master'
Your branch is up to date with 'origin/master'.
```

## 6. fetch해뒀던 upstream/master를 checkout했던 master 브랜치로 병합시킨다.
```
❯ git merge upstream/master
Updating ba2a115..375a378
Fast-forward
frontend/package.json       |  1 +
frontend/src/pages/index.js | 54 +++++++++++++++++++++++++++++++++++++++++++++++++++++-
2 files changed, 54 insertions(+), 1 deletion(-)
```
정상적으로 변경 내역이 반영되었음을 확인할 수 있다.  

#

#### Github Documents
> https://help.github.com/en/articles/syncing-a-fork
