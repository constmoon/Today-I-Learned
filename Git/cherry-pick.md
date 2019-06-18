# cherry-pick

* 다른 브랜치의 전체 commit 내역을 가져오지 않고 특정 commit 내역만을 가져오는 것이다(commit 하나만 rebase하는 것). 
*  다른 브랜치에 있는 특정 commit을 현재 브랜치에 반영하고 싶을 때 사용한다. 
* 체리 나무에 달려 있는 체리를 하나씩 골라 따듯이, 커밋들리 달려 있는 브랜치에서 필요한 커밋들을 골라서 가져오기 위한 개념으로 이해함.

```
git cherry-pick COMMIT_ID
```

cherry-pick은 commit을 가져오는 게 아니라, 가져올 commit을 새로 만들어서 현재 브랜치에 덧붙이는 작업이다. 즉, 브랜치에 붙는 commit의 id는 달라진다.
