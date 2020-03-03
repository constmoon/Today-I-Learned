# create-react-app 없이 리액트 프로젝트 생성하기

create-react-app 없이 리액트 프로젝트를 생성해보며 각 툴체인이 어떤 역할을 하는지 살펴보자.



### Package Manager

프로젝트를 생성할 디렉토리를 만들고 package manager를 통해 프로젝트를 생성한다. 

```bash
$ mkdir my-react-app && cd $_
$ yarn init -y
```

`-y` 옵션은 기본 설정값으로 `package.json`을 생성한다. `package.json` 은 아래와 같을 것이다.

```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}

```



### Module Bundler - Webpack

모듈 번들러란 여러 개의 나누어져 있는 파일들을 하나의 파일로 만들어주는 라이브러리를 의미한다. Webpack은 그 중 하나이며, 자바스크립트, 스타일시트, 이미지 등을 모듈로 로딩하고 하나의 파일로 묶어주는 역할을 한다.

![img](https://miro.medium.com/max/3280/1*SL6RVjoNQaUdii2Qh9XeZg.png)

[출처](https://medium.com/@paul.allies/webpack-managing-javascript-and-css-dependencies-3b4913f49c58)

```shell
$ yarn add -D webpack webpack-cli
```

`-D` 옵션을 넣어 package.json의 dependencies가 아닌 devDependencies에 저장한다. `devDependencies`는 개발 시에만 사용하는 개발용 의존 패키지를 명시한다. Webpack와 같은 번들러는 개발 단계에서만 필요하고 배포할 필요는 없으므로 devDependencies에 포함시킨다.

```json
"devDependencies": {
  "webpack": "^4.42.0",
  "webpack-cli": "^3.3.11"
}
```

설치가 완료되면 package.json 파일에 build 명령어를 추가한다.

```json
"scripts" : {
  "build": "webpack"
}
```

프로젝트 폴더에 `webpack.config.js` 파일을 생성하고 추가적인 설정을 해준다.

```
my-react-app
├─ node_modules
├─ package.json
├─ src
│  └─ index.js
└─ webpack.config.js
```

```javascript
// weback.config.js
const path = require("path");

module.exports = {
  mode: "production",
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname + "/build"),
    filename: "bundle.js"
  },
};

```

* mode: production, development, none 3가지 옵션이 있으며, 해당값에 따라 내부 최적화 옵션을 따른다.
* entry: 앱의 시작점([entry point](https://webpack.js.org/concepts/entry-points))을 의미한다. 리액트 앱이 있는 위치를 설정하여 웹팩에게 빌드를 시작할 위치를 알려준다. 위의 경우 src/index.js 파일 기준으로 import 되어있는 모든 파일을 찾아 하나의 파일로 합치게 된다.
* output: 빌드된 번들 파일을 저장하는 설정값
* path: 번들 파일이 저장될 위치. `path.resolve` 를 사용하면 절대경로로 저장된다.
* filename: 번들 파일의 이름. `bundle.[hash].js`와 같이 `[hash]`를 추가하면 앱이 수정되어 다시 컴파일 될 때마다 웹팩에서 생성한 해시로 변경해주어 캐싱에 도움이 된다.

이제 package.json 파일에 빌드 명령어를 추가한다.

```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "build": "webpack"
  },
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11"
  }
}
```

터미널에서 `yarn build ` 를 하면 output 설정대로 build 디렉토리에 bundle.js가 생성되었음을 볼 수 있다.



### Babel

React 컴포넌트들은 JavaScript ES6+ 문법과 JSX 문법으로 작성된다. 이 코드를 그대로 쓰는 경우 지원하지 않는 브라우저에서는 코드가 동작하지 않으므로 Babel을 사용하여 변환을 해줘야 한다.

```shell
$ yarn add -D @babel/core babel-loader @babel/preset-env @babel/preset-react
```

- @babel/core : babel 사용을 위한 코어 라이브러리
- babel-loader : Webpack을 사용할 때 babel을 적용하기 위한 라이브러리
- @babel/preset-env : JavaScript ES6 코드를 ES5로 변환해주는 라이브러리
- @babel/preset-react : JSX 코드를 JavaScript 코드로 변환시켜 주는 라이브러리

##### `import 'React' from 'react'`를 하는 이유도 여기에 있다!

JSX 문법은 바벨의 [transform-react-jsx](https://babeljs.io/docs/en/babel-plugin-transform-react-jsx)에 의해서 변환이 된다. 여기서 `React.createElement()`로 변환이 되는데, 이때 React를 못 찾는 문제를 해결하기 위해 React도 import를 해야한다.

바벨 설치가 다 되었으면  `.balbelrc` 파일을 만들고 다음과 같은 설정을 해준다.

```
my-react-app
├─ node_modules
├─ package.json
├─ src
│  └─ index.js
├─ webpack.config.js
└─ .babelrc
```

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

이제 바벨이 프리셋(preset) 플러그인을 사용할 수 있게 되었다.



### CSS 컴파일

Webpack을 통해 CSS를 컴파일하도록 아래 패키지를 설치한다.

```shell
$ yarn add -D css-loader style-loader mini-css-extract-plugin
```

* css-loader: 스타일시트를 Webpack이 읽어들일 수 있는 자바스크립트 파일로 변환해준다.
* style-loader: 자바스크립트로 변경된 CSS를 `<style>` 태그에 삽입하여 DOM에 추가해준다.
* mini-css-extract-plugin: style 태그 대신 CSS 파일로 만들고 싶은 경우에 사용한다.



### Webpack - loader

Webpack은 기본적으로 자바스크립트 파일만 인식하지만, 로더를 사용하면 [다른 타입의 파일도 모듈로 변환할 수 있다](https://webpack.js.org/concepts/modules). 

모듈 번들링시 바벨을 사용하고 CSS 파일을 인식하도록 아래와 같이 작성한다.

```js
const path = require("path");

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname + "/build"),
    filename: "bundle.js"
  },
  module: {
		rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: "/node_modules/",
        use: ["babel-loader"],
			},
      {
        test: /\.css$/,
        use: [
          {
            loader: "css-loader",
            options: {
              modules: true,
              localsConvention: "camelCase",
            }
          }
        ]
      }
    ]
	},
};
```

* `node_modules` 디렉토리를 제외한 자바스크립트 파일을 찾은 다음 `babel-loader`를 통해 바벨을 사용해 바닐라 자바스크립트로 변환한다. 바벨은 `.babelrc` 파일에서 설정 내용을 읽는다.

* CSS 파일을 찾고 `style-loader`와 `css-loader`로 CSS를 처리한다. 그 다음 `css-loader`에게 CSS 모듈과 카멜 케이스(camel case)를 사용하도록 한다. 

* css-loader에 옵션을 적용하면 이제 `import Styles from './styles.css'` 또는 `import { style1, style2 } from ''./styles.css'`와 같은 문법으로 스타일 정의를 할 수 있다.

  ```jsx
  <div className={Style.style1}>Hello World</div>
  // or
  <div className={style1}>Hello World</div>
  ```

  kebab-case로 작성된 CSS 클래스를 camelCase로 정의해 가져올 수 있다.

  ```css
  .home-button {...}
  ```

  ```jsx
  import { homeButton } from './style.css'
  ```



이렇게 번들된 파일을 브라우저에서 실행하기 위해 `public/index.html`을 생성한다.

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>My React App</title>
</head>
<body>
  <div id="root"></div>
  <noscript>
    You need to enable JavaScript to run this app.
  </noscript>
</body>
</html>
```

Webpack에서 html을 빌드할 수 있도록 플러그인을 설치한다. 

```shell
$ yarn add -D html-webpack-plugin clean-webpack-plugin
```

 설치 후 webpack.config.js 파일에 아래와 같이 html 관련 코드를 추가해준다.

```json
const path = require("path");
const HtmlWebPackPlugin = require("html-webpack-plugin");
module.exports = {
  plugins: [
    new HtmlWebPackPlugin({
      template: "./public/index.html",
      filename: "index.html",
    }),
  ]
}
```

* template: template을 기준으로 번들링 될 때마다 새로운 html 파일을 생성한다. 이때 번들된 자바스크립트 파일을 자동으로 삽입해주므로, 번들 파일의 이름이 변경될 때마다 index.html 파일의 script를 손대지 않아도 된다.
* filename: 생성할 html 파일 이름



### Creating React Components

React 라이브러리를 설치하고 컴포넌트를 생성한다.

```shell
$ yarn add react react-dom 
```

이제 필요한 도구를 모두 설정했다. 현재까지 package.json은 아래와 같다.

```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "build": "webpack"
  },
  "dependencies": {
    "react": "^16.13.0",
    "react-dom": "^16.13.0"
  },
  "devDependencies": {
    "@babel/core": "^7.8.6",
    "@babel/preset-env": "^7.8.6",
    "@babel/preset-react": "^7.8.3",
    "babel-loader": "^8.0.6",
    "css-loader": "^3.4.2",
    "html-webpack-plugin": "^3.2.0",
    "mini-css-extract-plugin": "^0.9.0",
    "style-loader": "^1.1.3",
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11"
  }
}
```

React 설치 후 `src/index.js`에 아래와 같은 내용을 입력한다.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

import App from './components/App';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

앱의 루트가 되는 App 컴포넌트를 보여주기 위해서 `ReactDOM.render` 함수를 사용한다. 첫번째 파라미터는 렌더링을 할 대상 컴포넌트이고, 두번째 파라미터는 컴포넌트를 어떤 DOM에 그릴지 정해준다. 해당 DOM은 `public/index.html` 파일에서 찾을 수 있다.

```html
<body>
  <div id="root"></div>
  <noscript>
    You need to enable JavaScript to run this app.
  </noscript>
</body>
```

앱의 메인화면을 나타내는 `components/App.js`를 만들고 내용을 입력한다.

```jsx
import React from 'react';

const App = () => {
  return (
    <h1>Hello, React</h1>
  );
};

export default App;
```



### Live Dev Server 

이전까지는 소스코드를 수정할 때마다 빌드 명령어를 직접 입력하여 빌드파일을 생성했다. 이런 불편함을 없애기 위해 소스코드가 수정될 때마다 알아서 웹팩이 빌드해주는 webpack-dev-server가 있다. 

```shell
$ yarn add -D webpack-dev-server
```

설치 후 webpack.config.js에 서버 설정을 추가한다.

```js
module.exports = {
  ...
  devServer: {
    contentBase: path.resolve("./build"),
    index: "index.html",
    port: 3000
  },
  ...
}
```

package.json에 서버를 실행시키는 scripts도 추가한다.

```json
"scripts": {
	"build": "webpack",
	"start": "webpack-dev-server"
}
```

이제 `yarn start`를 실행하고 브라우저로 `http://localhost:3000 `에 접속하면 빌드된 모습을 확인할 수 있다. 출력될 내용을 변경하고 저장하면 변경사항이 바로 빌드가 된다.



이렇게 모든 설정이 끝났다! 전체 디렉토리와 package.json은 아래와 같다.

```
my-react-app
├─ build
├─ node_modules
├─ public
│  └─ index.html
├─ src
│  ├─ components
│  │  └─ App.js
│  ├─ style.css
│  └─ index.js
├─ .babelrc
├─ .gitignore
├─ package.json
├─ webpack.config.js
└─ yarn.lock
```

```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "build": "webpack",
    "start": "webpack-dev-server"
  },
  "dependencies": {
    "react": "^16.13.0",
    "react-dom": "^16.13.0"
  },
  "devDependencies": {
    "@babel/core": "^7.8.6",
    "@babel/preset-env": "^7.8.6",
    "@babel/preset-react": "^7.8.3",
    "babel-loader": "^8.0.6",
    "css-loader": "^3.4.2",
    "html-webpack-plugin": "^3.2.0",
    "mini-css-extract-plugin": "^0.9.0",
    "style-loader": "^1.1.3",
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3"
  }
}
```



### .gitignore

GitHub 등 remote repository에 push 시에 업로드하지 않을 파일들을 명시한다.

```
# dependencies
/node_modules

# production
/build

# misc
.DS_Store
```

* node_modules: 패키지 매니저로 설치한 모듈 정보는 package.json에 담겨있기 때문에 모듈이 git으로 관리할 필요가 없다.
* build: 자동으로 생성되는 빌드 파일의 경우 git으로 관리할 필요가 없다.
* .DS_Store: Desktop Services Store의 약자로 애플에서 제공하는 해당 폴더에 대한 메타 데이터이다.

[gitignore.io](https://www.gitignore.io)에서는 개발 툴마다 .gitignore를 생성해주기도 한다.



### 결론

create-react-app 없이 리액트 프로젝트를 생성하기 전에 제일 궁금했던 부분은 `리액트 컴포넌트가 어떻게 웹 브라우저에서 작동하는지`였다. 분명 Webpack과 Babel을 사용하는 것 같은데, 왜 필요하고 어떻게 쓰이는 건지 제대로 알고 싶었다.

정리하자면 여러 개의 자바스크립트 파일을 하나의 파일로 묶어 요청 수를 줄이고, 최신 자바스크립트 문법을 모든 브라우저에서 사용하기 위해 모듈 번들러를 사용한다. 모듈 번들러는 자바스크립트 코드를 압축하고 최적화 할 수 있기 때문에 로딩 속도를 높여준다.

모듈 번들러 라이브러리인 Webpack은 모든 파일을 모듈로 관리한다. 하지만 Webpack은 자바스크립트 파일만 읽어 올 수 있기 때문에, 스타일시트나 이미지 등을 Webpack이 읽을 수 있는 자바스크립트로 변경해야 한다. Webpack이 이해 할 수 있는 모듈로 변경해 주는 게 loader의 역할이다.

Webpack은 로더(babel-loader, css-loader...)를 사용하여 여러 파일을 모듈로 변환한다. 이때 entry로 설정한 index.html부터 시작하여 모든 변환을 진행하고, 이들을 묶은(번들링한) 파일을 생성한다. 왜 개발자들이 `웹팩으로 만다`는 표현을 사용하는지  이해할 수 있었다. 

Babel은 리액트의 JSX 문법 및 ES6+ 자바스크립트 문법이 오래된 버전의 브라우저에서도 동작할 수 있도록 ES5 버전의 자바스크립트 문법으로 변환해준다. Webpack은 babel-loader을 통해 Babel의 변환 기능을 사용하고, 변환 작업은 Babel이 지정한 preset을 이용하여 진행한다. 

이외에도 다양한 Webpack config option들이 있다. 다른 사람들은 어떤 경우에 어떤 플러그인들을 사용하는지 찾아보는 게 다음 질문이 될 것이다.

