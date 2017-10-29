## 디렉토리 구조

```
my-app/
  README.md
  node_modules/
  package.json
  .gitignore
  yarn.lock
  public/
    index.html
    favicon.ico
    manifest.json
  src/
    index.css
    index.js
```

프로젝트가 정상적으로 구동하기 위해서는 **다음 파일들의 이름은 변경하시면 안됩니다**:

* `public/index.html` 페이지 템플릿입니다.
* `src/index.js` 메인이 되는 자바스크립트 파일입니다.

다른 파일들은 지우거나 이름을 변경해도 괜찮습니다.

`src`디렉토리 속에 다른 디렉토리를 만드셔도 괜찮습니다. `src`디렉토리안에 있는 파일들만 Webpack을 통해 프로세스됩니다.<br>
**모든 JS나 CSS 파일들은 `src`디렉토리안에** 위치해야 합니다. 그렇지 않으면 Webpack 프로세스에 포함되지 않습니다.

## 사용 가능한 명령어

현재 프로젝트에서 사용 가능한 명령어는 다음과 같습니다.

### `npm start`

어플리케이션을 Development 모드로 구동시킵니다.<br>
브라우저에서 [http://localhost:3000](http://localhost:3000)을 열어서 확인할수 있습니다.

수정을 하면 페이지가 새로고침됩니다.<br>

### `npm test`

Watch Mode에서 테스트를 구동시킵니다.<br>

### `npm run build`

배포를 위해 어플리케이션을 준비합니다. `build`디렉토리에 파일들이 준비됩니다.<br>

### `npm run eject`

**Note: 주의하세요. 한번 실행하면 되돌릴수 없습니다. 자신없으면 하지마세요.**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (Webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## 지원되는 자바스크립트 기능 및 Polyfills

현재 이 프로젝트는 가장 최신 자바스크립트 표준을 지원합니다.<br>
[ES6](https://github.com/lukehoban/es6features)를 포함해 추가적으로 다음과 같은 기능들은 사용 가능합니다.

* [Exponentiation Operator](https://github.com/rwaldron/exponentiation-operator) (ES2016).
* [Async/await](https://github.com/tc39/ecmascript-asyncawait) (ES2017).
* [Object Rest/Spread Properties](https://github.com/sebmarkbage/ecmascript-rest-spread) (stage 3 proposal).
* [Dynamic import()](https://github.com/tc39/proposal-dynamic-import) (stage 3 proposal)
* [Class Fields and Static Properties](https://github.com/tc39/proposal-class-public-fields) (part of stage 3 proposal).
* [JSX](https://facebook.github.io/react/docs/introducing-jsx.html) and [Flow](https://flowtype.org/) syntax.

Note: **현재 프로젝트는 ES6 [polyfills](https://en.wikipedia.org/wiki/Polyfill)중 소수만 제공합니다.**:

* [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) via [`object-assign`](https://github.com/sindresorhus/object-assign).
* [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) via [`promise`](https://github.com/then/promise).
* [`fetch()`](https://developer.mozilla.org/en/docs/Web/API/Fetch_API) via [`whatwg-fetch`](https://github.com/github/fetch).

## 에디터에서 Syntax 인식하기

[Babel documentation page](https://babeljs.io/docs/editors)에 가시면 많이 쓰이는 텍스트 에디터들에서 Syntax 표기를 위한 방법들이 나와있습니다.

## 코드 스타일 검수

현재 프로젝트에서는 Prettier라는 코드 스타일 관련 도구를 사용합니다. 이 도구를 사용하여 통일된 코드 스타일이 적용되도록 자동으로 수정해줍니다. [Prettier's GitHub page](https://github.com/prettier/prettier)에 가시면 더 많은 정보를 보실수 있습니다.

## 필요한 패키지 설치하기

현재 프로젝트에는 React와 ReactDOM은 이미 설치되어 있습니다. 그 외에도 몇가지 추가적으로 설치되어 있습니다. 만약 다른 패키지를 설치하고 싶으시다면, 다음과 같이 하시면 됩니다. (예: React Router 설치)

```sh
npm install --save react-router
```

`yarn`를 이용하여 설치해도 됩니다:

```sh
yarn add react-router
```

`react-router`뿐만 아니라 다른 패키지도 가능합니다.

## 컴포넌트 import하기

현재 프로젝트는 ES6 모듈을 지원합니다. `require`나 `module.exports`를 사용해도 되지만, [`import`나 `export`](http://exploringjs.com/es6/ch_modules.html)를 사용하시는게 더 좋습니다.

예제:

### `Button.js`

```js
import React, { Component } from 'react';

class Button extends Component {
  render() {
    // ...
  }
}

export default Button; // Don’t forget to use export default!
```

### `DangerButton.js`


```js
import React, { Component } from 'react';
import Button from './Button'; // Import a component from another file

class DangerButton extends Component {
  render() {
    return <Button color="red" />;
  }
}

export default DangerButton;
```

[default와 named exports의 차이점](http://stackoverflow.com/questions/36795819/react-native-es-6-when-should-i-use-curly-braces-for-import/36796281#36796281)을 분명히 알고 사용하세요. 아주 흔한 실수입니다.

모듈이 한가지만 export할때에는 default imports와 exports를 사용하시기를 추천합니다 (예, 컴포넌트). `export default Button`이나 `import Button from './Button'`을 사용하는 것과 같습니다.

Named exports는 유틸리티 모듈과 같이 여러가지의 함수를 export하는 상황에서 유용합니다.

ES6 모듈에 대해 더 알고 싶으시다면 다음 링크들을 보세요:

* [When to use the curly braces?](http://stackoverflow.com/questions/36795819/react-native-es-6-when-should-i-use-curly-braces-for-import/36796281#36796281)
* [Exploring ES6: Modules](http://exploringjs.com/es6/ch_modules.html)
* [Understanding ES6: Modules](https://leanpub.com/understandinges6/read#leanpub-auto-encapsulating-code-with-modules)

## Adding a Stylesheet

현재 프로젝트 구성은 [Webpack](https://webpack.js.org/)을 사용하여 asset을 관리합니다. Webpack은 자바스크립트의 `import`로 가능한 기본 기능보다 한단계 더 나아가 추가적인 기능을 제공합니다. 어떤 자바스크립트 파일에 CSS 파일이 필요하다면 **해당 자바스크립트 파일에서 CSS파일을 import하시면 됩니다.**

### `Button.css`

```css
.Button {
  padding: 20px;
}
```

### `Button.js`

```js
import React, { Component } from 'react';
import './Button.css'; // Tell Webpack that Button.js uses these styles

class Button extends Component {
  render() {
    // You can use them as regular CSS styles
    return <div className="Button" />;
  }
}
```

## CSS 포스트 프로세스

현재 프로젝트는 CSS를 minify하고 vendor prefix를 자동으로 붙여주는 기능들이 있습니다.

For example, this:

```css
.App {
  display: flex;
  flex-direction: row;
  align-items: center;
}
```

는 다음과 같이 자동으로 변환됩니다.

```css
.App {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
      -ms-flex-direction: row;
          flex-direction: row;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
}
```

## 이미지, 폰트, 파일 추가하기

Webpack을 사용하여 다른 파일들도 CSS와 비슷하게 다룰수 있습니다.

**자바스크립트 모듈에 바로 `import`할수 있습니다.**

```js
import React from 'react';
import logo from './logo.png'; // Tell Webpack this JS file uses this image

console.log(logo); // /logo.84287d09.png

function Header() {
  // Import result is the URL of your image
  return <img src={logo} alt="Logo" />;
}

export default Header;
```

다음과 같이 사용하셔도 됩니다:

```css
.Logo {
  background-image: url(./logo.png);
}
```
