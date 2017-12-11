# Day 1. 기본기 익히기

## React란?

React는 User Interface를 구축하기위해 사용하는 굉장히 효과적이고 유연한 자바스크립트 라이브러리입니다.

**MVC 기억나시죠? React는 MVC에서 따지자면, V에 해당하는 작업을 수행해주는 라이브러리입니다. 전체적인 프론트엔드 어플리케이션을 관리하는것이 아니라 특정 부분에 관련한 도구라서 프레임워크가 아닌 라이브러리라고 부릅니다. 주목할만한 점은, 첫째로 컴포넌트 기반이라는 것 그리고 둘째로는 Virtual DOM을 사용한다는 것입니다.**

React에는 여러가지의 컴포넌트들이 있지만, 우리는 우선 `React.Component`라는 React의 하위 클래스부터 시작하겠습니다.

```javascript
class UserProfile extends React.Component {
  render() {
    return (
      <div className="user-profile">
        <h1>사용자 이름: {this.props.name}</h1>
      </div>
    );
  }
}

// 컴포넌트 사용 방법: <UserProfile name="허근행" />
```

희한하게 생겨먹은 HTML같이 생긴 녀석은 잠시후에 가르쳐 드리겠습니다. 우리의 컴포넌트는 React에게 우리가 무엇을 보여주고 싶은지를 전달해줍니다. 그러면 React는 해당 컴포넌트를 보여주고 데이터가 바뀌면 효과적으로 컴포넌트를 업데이트시켜줍니다.

위의 예제에서 `UserProfile`은 **React component class**, 혹은 **React component type**입니다. 각 컴포넌트는 `props`라고 부르는 parameter들을 받을 수 있고, `render` 메소드를 통해 보여줘야하는 view를 return합니다.

`render` 메소드는 무엇을 보여주고 싶은지에 대한 상세 내용을 return해야 하고, React는 해당 상세 내용을 받아 화면에 보여주는 일을 합니다. 조금 더 정확히 이야기하자면, `render` 메소드는 **React element**를 return합니다 (무엇을 보여줘야하는지에 대한 정보를 가지고 있는 객체). 대부분의 React 개발은 **JSX**라는 특별한 파일 형식을 사용하고, 우리가 더 편하고 보기 좋게 쓸수 있도록 도와줍니다. `<div />`와 같은 JSX 코드는 빌드 과정을 통해 `React.createElement('div')`로 변환됩니다. 위의 예제는 다음 코드와 비슷하게 변환된다고 생각하시면 됩니다.

```javascript
return React.createElement('div', { className: 'user-profile' },
  React.createElement('h1', null, '사용자 이름: ', this.props.name)
);
```

`createElement`에 대해 더 알고 싶으시면, 공식 [API](https://reactjs.org/docs/react-api.html#createelement) 문서를 보시면 됩니다. 하지만, 우리는 JSX를 쓸 계획이기 때문에, 굳이 안보셔도 됩니다.

JSX 속에서는 중괄호를 이용하여 자바스크립트 코드를 적을수 있습니다. 모든 React element는 변수에 할당도 할수있고 여기저기서 사용할수 있는 단순 자바스크립트 객체입니다.

`UserProfile` 컴포넌트는 단지 관련 DOM 컴포넌트만 만들어서 보여주지만, 모든 React 컴포넌트는 다른 컴포넌트에서도 쉽게 사용할수 있습니다 (예, `<UserProfile />`). 각각의 컴포넌트는 독립적으로 작동하도록 캡슐화가 되어있기에, 복잡하고 난해한 UI를 단순하고 간단한 컴포넌트들로 구성하여 만들수 있도록 해줍니다.

## 시작하기

`src/index.js` 파일을 보시면 우리는 현재 다음과 같이 세가지의 컴포넌트가 있습니다.

* Square
* Board
* Game

`Square` 컴포넌트는 단지 `<button>`을 보여줍니다. `Board`는 9개의 `Square`를 보여줍니다. `Game`은 `Board`와 몇가지 추가적인 placeholder들을 보여줍니다. 아직 각각의 컴포넌트들은 단순하게 보여주기만할뿐 별다른 기능은 없습니다.

## `props`를 통해 데이터 전달하기

연습삼아 한번 `Board` 컴포넌트에서 `Square` 컴포넌트로 데이터를 전달해보겠습니다.

`Board` 컴포넌트의 `renderSquare` 메소드를 다음과 같이 바꿔보세요.

```javascript
class Board extends React.Component {
  renderSquare(i) {
    return <Square value={i} />;
  }
```

그리고 `Square` 컴포넌트의 `render` 메소드에서 `{/* TODO */}`부분을 `{this.props.value}`로 바꿔보세요.

```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {this.props.value}
      </button>
    );
  }
}
```

이제 화면의 각 네모안에 숫자들이 보일겁니다.

## Interactive Component

네모칸을 클릭했을때, X 마크가 생기도록 만들어 보겠습니다. `Square` 컴포넌트의 `render` 메소드를 우선 다음과 같이 바꿔보세요.

```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

네모를 클릭하면, 브라우저에서 경고 메시지가 뜰겁니다.

---

**NOTE: `onClick={alert('click')}`과 `onClick={() => alert('click')}`의 차이점**

우리는 `onClick`이라는 `prop`에게 **클릭되었을때 실행시켜야할 함수**를 넘겨주어야 합니다. `onClick={alert('click')}`이라고 넘겨준다면, 이 코드는 `onClick`이라는 `prop`이 전달되는 즉시 경고창을 실행시켜주게 됩니다.

---

React 컴포넌트들은 `state`(상태?)을 갖고 있을수 있습니다 (예, 생성자 함수에서 `this.state` 선언). `state`이란 해당 컴포넌트에게만 속하고 외부로 유출되지 않는 정보입니다. 네모칸의 값을 `state`에 저장하고 네모가 클릭되었을때 값을 바꿔줘보도록 하겠습니다.

우선 생성자 함수에서 `state`을 설정해주어야 합니다.

```javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => alert('click')}>
        {this.props.value}
      </button>
    );
  }
}
```

이제 `Square` 컴포넌트의 `render` 메소드에서 현재 상태를 보여주고 클릭했을때 바꿔주는 코드를 추가해보겠습니다.

* `<button>`태그 안에서 `this.props.value`를 `this.state.value`로 바꿔주세요.
* `() => alert()` 이벤트 리스너를 `() => this.setState({ value: 'X' })`로 바꿔주세요.

다음과 같이 코드가 완성되어야 합니다.

```javascript
class Square extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: null,
    };
  }

  render() {
    return (
      <button className="square" onClick={() => this.setState({value: 'X'})}>
        {this.state.value}
      </button>
    );
  }
}
```

`setState`이 실행되면, 컴포넌트를 업데이트하는 작업이 예약되고, React는 입력된 `state`를 이용하여 해당 컴포넌트와 하위 컴포넌트들을 다시 보여주는 작업을 시작합니다. 해당 컴포넌트가 다시 화면에 그려지면, `this.state.value`는 `'X'`로 지정되고 화면에 X 마크를 보여줍니다.

이제 네모칸을 클릭하면 X 마크가 뜰겁니다.

## React 개발을 위한 도구

[Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)이나 [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/) 확장 프로그램으로 만들어진 React 개발을 위한 도구들이 있는데, 이것들을 이용하면 React 컴포넌트의 Tree구조를 쉽게 검사해볼수 있습니다. 그리고 각 컴포넌트들의 `state`와 `props`를 살펴볼수 있습니다.

설치를 하신후에, 화면에 보이는 UI에 오른쪽 클릭을 하시고 "검사"를 눌러 개발자 도구를 열어주세요. 그리고 가장 오른쪽에 위치한 `React` 탭을 선택하세요.
