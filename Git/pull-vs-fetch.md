# Pull과 Fetch의 차이점(Pull / Fetch, Merge, Rebase)
내 계정에 Fork한 저장소(repository)에서 작업하고 있는데, 원본 저장소에 업데이트(새로운 commit)가 생겼다. 업데이트된 내역을 내 저장소에 추가하려고 하는데 pull을 하자니 Already up to date가 떴다. 분명 예전에도 같은 문제를 겪었고, 어떻게 검색해서 찾은 것 같은데... 이제는 확실하게 기억해야겠다.

## 공통점
* 원격 저장소(Original Repository)의 소스를 로컬 저장소(Forked Repository)로 가져온다.

## 차이점
### Pull
* 원격 저장소의 변경 내역을 가져와서 로컬 저장소의 내용을 변경하는 병합(Merge)까지 이루어진다.
* 충돌하는 변경이 없을 경우 자동적으로 병합되지만, 충돌이 있을 경우에는 충돌 부분을 수동으로 해결해야한다.
* pull = fetch + merge

### Fetch
* 원격 저장소의 변경 내역을 가져오되, 로컬 브랜치에 반영하지 않는다. 즉, 현재 작업중인 소스들이 Merge 당할 일이 없다.
* 변경된 데이터를 '가져오기만 하는' 작업
* Fetch로 가져온 내용은 `FETCH_HEAD` 라는 이름으로 임시 저장된다.
* `git diff FETCH_HEAD` 를 통해 새로 가져온 내용과 로컬의 내용을 비교할 수 있다.
* fetch 후에는 `merge`와 `rebase`를 목적에 맞게 사용하여 최종적으로 내용을 통합한다.
* `merge` : branch A와 master의 커밋들을 합칠 때, branch A와 master를 병합하는 커밋이 master에 HEAD로 새로 추가된다. 마지막 커밋을 한 번에 통합하는 과정으로써 마지막 커밋 이전의 이력은 남지 않는다.
* `rebase`: branch A와 master의 커밋들을 합칠 때, branch A를 베이스(base)로 커밋을 재정렬한다. 모든 커밋 내용이 master에 남게 된다.  
#

`pull`은 작업 수가 적기 때문에 간단하게 사용할 수 있고, `fetch`는 변경 내용 등을 확인할 수 있으므로 예기치 못한 변경 사항의 반영을 방지할 수 있다(안전). 용도에 맞게 사용하자.
