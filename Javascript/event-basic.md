# Javascript Event

## 이벤트(event)
이벤트란 웹을 사용하는 동안 브라우저에서 발생할 수 있는 사건을 의미한다. 
- 사용자의 어떤 액션(마우스 클릭, 키보드 입력)에 의해서 이벤트가 발생하는 경우, 특정 로직을 수행하기 위해서 해당 이벤트와 특정 메소드를 연결해서 사용한다. 
- e.g. 마우스 버튼 클릭 >  이벤트 발생 > 미리 준비해놓은 코드 호출(실행)
- 사용자의 입력에 따라 추가적인 동작을 구현해야할 때 **이벤트를 등록한다**고 한다. 

## 이벤트 리스너(event listener)
이벤트 리스너란 이벤트가 발생했을 때 그 처리를 담당하는 함수를 가리키며, 이벤트 핸들러(event handler)라고도 한다.
- 지정된 타입의 이벤트가 특정 엘리먼트에서 발생하면, 브라우저는 그 요소에 등록된 이벤트 리스너를 실행시킨다.
- 작성된 이벤트 리스너는 먼저 해당 객체나 엘리먼트에 등록되어야만 호출될 수 있다.

## 이벤트 버블링(event bubbling)
특정 엘리먼트에서 이벤트가 발생했을 때, 해당 이벤트가 상위 엘리먼트들에게 전달되는 특성을 의미한다.

```html
<main onclick="alert('main')">
        <div id="box" onclick="alert('div')">
                <button type="button" onclick="alert('button')">button</button>
        </div>
</main>
```
버튼을 클릭해도 클릭 이벤트가 버블링되어 div과 main도 출력되는 것을 볼 수 있다.  
`stopPropagation()` 메소드를 호출하면 상위 엘리먼트에 이벤트가 가지 않도록 막을 수 있다.

## target vs currentTarget
#### `target`  
- 이벤트를 실제로 발생시킨 element(이벤트가 일어난 곳)  
- input의 체크박스를 클릭할 경우 target과 currentTarget의 차이를 보면 다음과 같다
```html
<ul id="todolist">
    <li><input type="checkbox"></li>
</ul>
<script>
var todolist = document.getElementById('todolist');
todolist.onclick = function(e){
        console.log(e.target);    // input
}
</script>
```
 
- ul에 이벤트를 할당했지만 이벤트가 버블링되어 실행되었다. 이럴 때 실제 발생된 input 이벤트가 target이다.
- delegation 구현시 target을 사용하는 이유도 실제 이벤트를 발생시킨 엘리먼트가 해당 엘리먼트 조건과 맞는지 확인하기 위함이다(e.target.className==="INPUT")

#### `currentTarget`
- 이벤트를 실제 바인딩한 element(실제로 이벤트가 걸려있는 위치)

```html
<ul id="todolist">
    <li><input type="checkbox"></li>
</ul>
<script>
var todolist = document.getElementById('todolist');
todolist.onclick = function(e){
        console.log(e.currentTarget);   // ul
};
</script>
```
currentTarget는 jQuery의 this과 같다. 즉, event를 발생기킨 element의 의미를 갖는다.
 
## event delegation
엘리먼트가 추가/삭제되었을 때 이벤트를 추가/삭제하는 작업을 관리하는 게 필요하다.  
상위에 있는 이벤트를 바인딩해서 이벤트 처리를 위임하고, 하위 엘리먼트에서는 이벤트가 버블링되면 이벤트를 분석하여 이벤트들을 관리하는 기법이다. 하위 엘리먼트에 각각 이벤트를 바인딩하지 않고도 상위 엘리먼트에서 하위 엘리먼트의 이벤트들을 제어한다.
```html
<ul id="todolist">
        <li>
                <input type="checkbox">
                <span>할 일</span>
        </li>
</ul>
<script>
// 기존의 방법: input 개수마다 이벤트를 붙인다
var inputList = document.querySelectorAll('li input');
for(var i=0; i<inputList.length; i++){
        inputList[i].addEventListener('click', function(){
                alert('clicked');
        });
}

// delegation을 사용해 상위 요소에만 이벤트를 붙인다
var todoList = document.querySelectorAll('todolist');
todolist.addEventListener('click', function(e){
        if(e.target.checked){
                alert('clicked');
        }
})
</script>
```

* 별도로 하위 엘리먼트까지 가서 바인딩할 필요가 없다. 특히 새로운 엘리먼트가 추가되어 아이템이 많아질 때, 추가된 엘리먼트에 대해서 일일이 바인딩하지 않아도 된다.
* 하위 엘리먼트가 클릭이 될 때마다 이벤트 필터링을 거치게 된다. 만약 body를 기준으로 delegation을 하면 자주 일어나는 mouse, key 이벤트의 경우 필터 작업이 자주 돌아가 성능에 좋지 못하니, 가능하면 delegation을 하는 상위 엘리먼트의 영역이 작아야한다(범위가 너무 크면 큰 영역만큼 다양한 이벤트들을 필터링해야함)


## module pattern
자바스크립트를 한 파일에 global로 함수나 변수로 만들게되면, 다른 파일과 합쳤을 때(같이 로딩했을 때) 같은 이름으로 만들 확률이 높다 -> 겹치게 되면 덮어씌우는 등의 문제가 발생 -> 자바스크립트 코드를 모듈화하여 외부에 private하게 함수나 변수를 사용하도록 함

## 즉시 실행함수
함수를 만들자마자 바로 실행하는 것

```javascript
(function(){
    console.log("run");
})();
```

* 안에서만 사용하고 외부에서는 변수나 함수에 접근하게 하고 싶지 않을 경우 사용
* 함수나 변수가 scope 안에서만 실행되는 것을 이용

외부에 노출하고 싶은 함수나 메소드가 있다면 변수로 할당받아서 사용

```javascript
var Foo = (function(){
    function init(){
        ...
    }
    return{
        init: init
    }
})

Foo.init();
```

## template pattern
html문자(템플릿)를 데이터와 합쳐서 엘리먼트로 삽입하는 방법. html문자를 따로 관리하여 로직과 뷰 부분을 분리할 수 있다.

## initialization

```javascript
window.addEventListener('load', function(){ .. } );
```

`load`  
* load: html의 모든 리소스들이 다운로드된 후에(DOM이 완성되면) 발생하는 이벤트  
* 그러나 리소스가 많은 사이트의 경우, 사이트를 로딩하는 과정에서 이벤트에 대한 결과를 볼 수 없음

`DOMContentLoaded`  
* DOM이 로딩되었을 때 이벤트 접근 가능(이미지 등 무거운 리소스들을 기다릴 필요가 X)

DomContentLoaded를 항상 사용하는 건 아니다. loaded된 리소스들에 대해 접근이 필요할 때는 load를 사용해야한다(e.g. 스크립트로 이미지 사이즈를 늘리고 줄이는 등)


**바인딩과 할당?**
* 바인딩: 변수에 변수와 관련된 속성을 연관시키는 것
* 이벤트 바인딩: 이벤트를 속성에 연관(bind)시키는 과정
* 할당: 변수에 메모리 공간을 바인딩하는 과정
의미적으로 큰 차이는 없지만 일반적으로 이벤트를 지칭할 때는 바인딩이라는 용어를 쓰는듯하다.
