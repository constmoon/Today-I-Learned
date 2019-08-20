# SASS

- CSS 확장언어
- CSS preprocessor(전처리기)의 한 종류
- CSS의 문법이 잘못되거나 오타가 나면 이전에는 브라우저에서 알아서 처리했으나, 전처리 과정이 추가되면 에러메세지를 출력하기 때문에 사용한다.
- 반복적인 작업을 자동화할 수 있다.

## 장점

- 호환성이 좋다. css파일을 scss로 바꾸기만 하면 됨
- 많은 기업들이 사용하며 커뮤니티 또한 크다(이슈 검색에 유용)
- SASS를 개량한 여러 프레임워크들(gulp, grant, webpack등 다른 플러그인들과 조합하여 사용했을 때 효용성이 좋다)

## 명령어

### css

- sass [컴파일 할 파일명]: `sass input.scss`
- sass [컴파일 할 파일명] [내보낼 css 파일명]: `sass input.scss output.css`
- sass --watch [컴파일 할 파일명]:[내보낼 css 파일명]: scss 파일이 수정될 때마다 자동으로 컴파일을 진행한다.

### --style

- sass --style expanded: 공백을 포함하여 컴파일(기본값)
- sass --style compressed: 공백 없이 한줄에 컴파일(문법을 최소화)

### --source-map

- 기본적으로는 map파일을 사용한다.
- 보통 sass를 사용할 때는 파일을 쪼개서 사용하는 경우가 많기 때문에, source map을 사용하지 않으면 요소 검사시 스타일을 추적하기가 어렵다.
- 배포할 때는 source map을 제거해야 한다.

## @import

- CSS에서 import를 사용할 경우 추가 파일 리퀘스트가 발생하기도 하며, import된 css가 로딩될 경우 새로 렌더링을 해야하는 경우가 있어서 사용을 지양하기도 한다.
- 그러나 SASS에서는 import를 컴파일하는 과정에서 불러오기 때문에 여러 파일을 합쳐서 하나의 css 파일을 생성한다.
- 모듈별로 파일을 분리해서 css를 구조화할 수 있다.
- `_sprite.scss` 라는 파일의 경우, 언더바(_)는 해당 파일을 import 전용으로 사용하겠다는 것을 의미한다. 언더바와 확장자는 생략 가능.
- 여러 개의 파일을 불러올 경우 콤마(,)단위로 구분하여 어러 개의 파일을 불러올 수 있다.

## Nesting

- 자식 선택자를 부모 선택자 안에 중첩해서 사용할 수 있다.
- 부모 선택자를 반복해서 쓰지 않기 때문에 가독성이 뛰어나고 구조화된 코드를 작성할 수 있다.
- 그러나 지나치게 중첩을 사용할 경우 뎁스가 깊어져 가독성이 떨어지므로, 되도록이면 중첩을 줄이도록 한다(중첩 3뎁스 이상은 지양하는 것을 권장).

## &

- 블럭 안에 `&`를 넣게되면 차상위 selector를 가리킨다(js의 this와 유사).
- 부모 참조 셀렉터라고 부른다.
- 가상 클래스, 가상 요소 등에서도 사용할 수 있다.

```scss
// scss
a {
    font-weight: bold;
    text-decoration: none;
    &:hover{
        text-decoration: underline;
    }
}

// css
a {
    font-weight: bold;
    text-decoration: none;
}
a:hover {
    text-decoration: underline;
}
```

- & 앞에 셀렉터를 넣게되면 부모 셀렉터 앞에 해당 셀렉터를 추가한 것과 같은 결과가 나온다.

```scss
// scss
.latte {
    .cappuccino & {
        font-size: 11px;
    }
}

// css
.cappuccino .latte {
    font-size :11px;
}
```

## 변수

- SASS에서 변수를 선언할 때는 앞에 `$`를 붙이며, 변수와 변수는 콜론(`:`)으로 구분한다.
- 변수의 값 뒤에는 반드시 세미콜론(`;`)이 붙어야한다.

### 전역 변수

- 최상위에 변수를 선언을 하게되면 어떤 블록에서도 해당 변수를 참조할 수 있다.

```scss
// scss
$size: 12px;

.latte {
    width: $size;
    .americano {
        width: $size;
    }
}
.cappuccino {
    width: $size;
}

// css
.latte { width: 12px; }
.latte .americano { width: 12px; }
.cappuccino { width: 12px; }
```

### 지역 변수

- 최상단이 아니라 셀렉터 블록 안에 변수를 선언하게 되면 선언된 블록 안에서만 유효한 스코프를 가지게 된다.

```scss
// scss
.latte {
    $size: 12px;

    width: $size;
    .americano {
        width: $size;
    }
}
.cappuccino {
    width: $size;
}

// css (컴파일 에러)
ERROR: Undefined variable: "$size".
```

### 지역 변수(override)

- 블럭 안에서 전역으로 선언된 $size에 값을 할당하여 다른 값을 갖도록 할 수 있다.
- 지역변수의 우선도가 전역변수보다 높다는 것을 알 수 있다.

```scss
// scss
$size: 10px;

.latte {
    $size: 12px;
    width: $size;

    .americano {
        width: $size;
    }
}
.cappuccino {
    width: $size;
}

// css
.latte { width: 12px; }
.latte .americano { width: 12px; }
.cappuccino { width: 10px; }
```

## !global flag

- !global 플래그를 변수 뒤에 붙이면 블럭 안에서도 전역 변수로 선언하는 효과가 있다.

```scss
// scss
$size: 10px;

.latte {
    $size: 12px !global;
    width: $size;

    .americano {
        width: $size;
    }
}
.cappuccino {
    width: $size;
}

// css
.latte { width: 12px; }
.latte .americano { width: 12px; }
.cappuccino { width: 12px; }
```

## interpolation #{}

- 각각의 변수값이 셀렉터와 속성의 참조값으로 사용된다.
- 반복문, 조건문, 이미지 url, 문자열을 끼워넣을 때 등의 경우에 사용한다.

```scss
// scss
$name: desc;
$attr: border;

p.#{$name} {
    ${attr}-color: blue;
}

// css
p.desc {
    border-color: blue;
}
```

## 변수의 타입

- 숫자
  - 단위까지 포함된다
  - 1, 1.5, 10px
- 문자열
  - 따옴표가 없는 경우에도 문자로 인식한다
  - "foo", 'bar', baz
-색상
  - 이름, rgb, hex
  - blue, #04a3f9, rgba(255,0,0,0.5)
- 불리언
  - true, false
- 널
  - null
- 리스트
  - 1.5em 1em 0 2em, Arial, sans-serif
- 맵
  - (key1: value1, key2:value2)
- function 참조
  - round(1.5)

## @MIXIN

- SASS의 핵심 기능 중 하나
- 일종의 프리셋 같은 느낌으로, 믹스인 이름을 지정하고 스타일 구문을 작성한다.
- 사용할 때는 `@include [믹스인명]` 으로 해당 믹스인을 불러온다. 해당 믹스인이 치환되어 들어간다.

```scss
// scss
@mixin ellipsis-text {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    max-width: 100%;
}

.text {
    @include ellipsis-text;
    color: #f00;
}

// css
.text {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    max-width: 100%;
    color: #f00;
}
```

- 셀렉터까지 포함할 수도 있고, 다른 믹스인을 포함할 수도 있다.

```scss
@mixin silly-links {
    a {
        color: blue;
        background-color: red;
    }
}
@include silly-links

@mixin compound {
    @include highlighted-background;
    @include header-text;
}
@mixin highlighted-background { background-color: red; }
@mixin header-text { font-size: 20px; }
```

- 함수처럼 인자값을 전달받을 수도 있다. 인자값으로 하나의 믹스인을 여러가지 방법으로 활용할 수 있게 된다.

```scss
@mixin border($color, $width) {
    border: {
        color: $color;
        width: $width;
        style: solid;
    }
}
p { @include border(blue, 2px); }
```

- 인자에 기본값을 넣어 include시 생략할 수도 있다.

```scss
@mixin border($color, $width: 1px) {
    border: {
        color: $color;
        width: $width;
        style: solid;
    }
}

p { @include border(blue); }
```

## @content

- 이전에는 믹스인명을 호출해서 단순 치환하거나 인자값을 조금씩 변형하는 형태였다면, content 블럭의 경우 구문을 통으로 넘기는 형태이다.
- 주로 미디어 구문을 효율적으로 관리하기 위해 사용한다.

```scss
@mixin mq {
    @media all and (max-width: 600px) {
        @content
    }
}

.mixin_media {
    background-color: pink;
    @include mq {
        background-color: green;
    }
}

// css
.mixin_media {
    background-color: pink;
}
@media all and (max-width: 600px) {
    .mixin_media {
        background-color: green;
    }
}
```

## @EXTEND

- mixin은 css 상에서 존재하지 않는 믹스인 구문으로 미리 프리셋을 설정해둔 것이라면, extend의 경우 이미 정의된 css 룰셋을 가져와서 확장하는 개념이다.
- 구문을 그대로 가져오는 게 아니라, 셀렉터에 구문이 추가되는 것을 볼 수 있다.
- 믹스인을 사용하면 같은 스타일에 대해 내용이 반복되어 css가 길어지나, 익스텐드를 사용할 경우 공통되는 스타일에 대해서 한 번에 묶어버리므로 css 길이가 줄어드는 장점이 있다.
- 믹스인과 비교했을 때 조금 더 최적화되었다고 볼 수 있지만, 요소 검사시에 추적이 힘들거나 셀렉터를 한번에 묶어버리기 때문에 우선순위를 예측하기가 어려울 수 있다(셀렉터의 중첩이 일어남).

```scss
// scss
.americano {
    font-size: 12px;
    text-align: center;
    color: #fff;
    background-color: red;
}
.americano_ice {
    @extend .americano;
    background-color: blue;
}

// css
.americano,
.americano_ice {
    font-size: 12px;
    text-align: center;
    color: #fff;
    background-color: red;
}

.americano_ice { background-color: blue; }
```

## %PLACEHOLDER

- placeholder 셀렉터는 기존 css에 없는 것으로, 가상 룰셋을 정의할 때 사용한다.
- 공통되는 스타일을 묶어 가상의 룰셋을 만들고, 컴파일을 하고나면 css에는 노출되지 않는다.
- 익스텐드, 믹스인의 단점을 보완한다.

```scss
// scss
%water { 
    font-size: 12px;
    text-align: center;
    color: #fff;
}
.americano {
    @extend %water;
    background-color: red;
}
.americano_ice {
    @extend %water;
    background-color: blue;
}

// css
.americano,
.americano_ice {
  font-size: 12px;
  text-align: center;
  color: #fff;
}

.americano { background-color: red; }
.americano_ice { background-color: blue; }
```

- extend를 하게되면 selector가 ,로 연결되어 뭉치는 형태가 되므로 제약이 많은 편이다.
- 자식 선택자, 인접 선택자처럼 복잡한 형태의 셀렉터에서는 extend를 사용할 수 없다. 이런 경우에는 placeholder를 사용할 수밖에 없다.
