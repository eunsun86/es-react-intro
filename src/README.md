# Day 2. 조금 더 기본기 익히기

## 상위 컴포넌트로 `State` 올려주기

지금 현재의 코드에서 `Square` 컴포넌트의 `state`는 그 안에 갇혀있고 해당 컴포넌트이외의 그 누구도 알지 못합니다. 누가 게임에서 이겼는지를 판단하기 위해서는 `Square` 컴포넌트 각각의 `state`을 한군데로 모아서 계산해야 합니다.

단순하게 `Board` 컴포넌트가 `Square` 컴포넌트 각각의 `state`을 수집해서 판단해야 한다고 생각하실수도 있습니다. 그런 방법도 물론 불가능한 것은 아니지만, 그런 방식은 보통 유지/보수가 힘들고 이해하기 난해한 코드로 변질되는 경향이 있습니다.

가장 좋은 방법은 상위 컴포넌트인 `Board`에 `state`을 만들어 관리하는 것입니다. 그리고 `Board` 컴포넌트가 각각의 `Square`에게 무엇을 보여줘야할지를 명령하는 것입니다.

**만약 여러개의 Child Component로부터 혹은 Child Component끼리의 커뮤니케이션이 필요하다면, 해당 `state`에 대한 정보를 상위 컴포넌트로 올려주고 상위 컴포넌트가 다시 다른 Child Component에게 `props`를 이용하여 알려주면 됩니다.**

`state`을 상위 컴포넌트로 올려주는 방식은 `React`에서 아주 흔한 유지/보수 방식입니다. `Board` 컴포넌트에게 `constructor` 함수를 만들어주시고 각각의 `Square`에게 초기값으로 넘겨줄 정보들을 담고 있는 배열을 만들어주세요.

```javascript
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  renderSquare(i) {
    return <Square value={i} />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

우리는 `this.state.squares` 배열에 전체적인 보드판의 정보를 다음과 같은 예시처럼 입력할 예정입니다.
```javascript
[
  'O', null, 'X',
  'X', 'X', 'O',
  'O', null, null,
]
```

현재 `Board` 컴포넌트의 `renderSquare` 메소드는 다음과 같이 짜여 있습니다.
```javascript
renderSquare(i) {
  return <Square value={i} />;
}
```

보여주어야할 값을 받는 `value`라는 `prop`을 만들어주세요.
```javascript
renderSquare(i) {
  return <Square value={this.state.squares[i]} />;
}
```

이번에는 각각의 `Square` 컴포넌트가 클릭되었을때를 처리해주어야 합니다. 현재는 `Board` 컴포넌트가 보드판에 어떤 값들을 각각 보여주어야하는지에 대한 정보를 저장하고 있습니다. 그래서 `Square` 컴포넌트가 클릭되었을때, `Board` 컴포넌트가 소유하고 있는 보드판에 대한 `state`를 업데이트해주어야 합니다. 하지만 `Square` 컴포넌트는 `Board` 컴포넌트의 `state`를 직접적으로 수정할수는 없습니다.

이런 경우에 일반적인 패턴은, `Board` 컴포넌트가 각각의 `Square` 컴포넌트에게 클릭 핸들러 함수를 보내주는 것입니다. `renderSquare` 함수를 다음과 같이 수정해보세요.
```javascript
renderSquare(i) {
  return (
    <Square
      value={this.state.squares[i]}
      onClick={() => this.handleClick(i)}
    />
  );
}
```

이제 우리는 `Board`에서 `Square`로 두가지를 내려보내고 있습니다(`value` & `onClick`). 다음과 같이 `Square` 컴포넌트를 수정해보세요.
```javascript
class Square extends React.Component {
  render() {
    return (
      <button className="square" onClick={() => this.props.onClick()}>
        {this.props.value}
      </button>
    );
  }
}
```

이제 `Square` 컴포넌트가 클릭되었을때, `Board`에게 물려받은 `onClick`에 해당하는 함수가 실행됩니다.

**NOTE: React 컴포넌트에게 `onClick`이라는 `prop`은 특별한 기능이 있습니다. 이벤트 핸들러를 추가할때, `on*`이라는 패턴을 이용합니다.**

네모칸을 한번 클릭해보세요. 에러가 발생할겁니다. 왜냐하면 아직 `handleClick`이라는 함수를 만들어주지 않았기 때문입니다. 다음과 같이 `Board` 컴포넌트를 수정해보세요.
```javascript
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = 'X';
    this.setState({squares: squares});
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

이제 다시 네모칸을 클릭할수 있게 되었고, 각각의 보드판에 보여주는 정보는 `Board` 컴포넌트가 관리하게 수정되었습니다. `Board`의 `state`이 수정되면, 각각의 `Square`는 다시 rendering됩니다.

## 플레이어 순서 바꾸기

현재 우리 게임의 아주 큰 문제점은 한사람만 참여할수 있다는 점입니다. 한번 고쳐볼까요?

첫 순서는 X가 되도록 초기값을 설정해주세요. `Board` 컴포넌트의 `constructor`를 다음과 같이 수정해주세요.
```javascript
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true
    };
  }
```

매번 네모를 클릭할때마다, 플레이어를 바꿔주어야 합니다. 누구의 순서인지는 이제 `xIsNext`의 값을 통해 결정합니다. 그리고 매번 클릭을 할때마다, `xIsNext`의 값을 바꿔줌으로서 다음 순서를 알수 있습니다. `handleClick` 함수에 `xIsNext`를 업데이트하는 부분을 수정해주어야 합니다.
```javascript
handleClick(i) {
  const squares = this.state.squares.slice();
  squares[i] = this.state.xIsNext ? 'X' : 'O';
  this.setState({
    squares: squares,
    xIsNext: !this.state.xIsNext,
  });
}
```

이제 X와 O의 순서가 매번 바뀝니다. 이번에는 `Board` 컴포넌트의 `status`를 바꿔서 `render`해주도록 수정해주셔야 합니다. 그래야 다음 순서가 누구인지를 보여줄수 있겠죠.
```javascript
render() {
  const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

  return (
    // 나머지는 동일합니다.
```

위처럼 변경하고 나면, `Board` 컴포넌트는 아래와 같습니다.
```javascript
class Board extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      squares: Array(9).fill(null),
      xIsNext: true,
    };
  }

  handleClick(i) {
    const squares = this.state.squares.slice();
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]}
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}
```

## 승부 판별하기

누가 게임에서 이겼는지를 보여주는 기능을 추가해보겠습니다. 아래와 같은 Helper 함수를 파일에 추가해주세요.
```javascript
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

우리는 위의 Helper 함수를 `render` 메소드에서 사용하여 X와 O중에 누가 이겼는지를 보여줄겁니다.

`Board` 컴포넌트의 `render` 메소드를 아래와 같이 수정해주세요.
```javascript
render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      // 나머지는 동일합니다.
```

승리자가 결정되었을 경우, 더 이상 클릭을 할 수 없도록 다음과 같이 바꿔주어야 합니다.
```javascript
handleClick(i) {
  const squares = this.state.squares.slice();
  if (calculateWinner(squares) || squares[i]) {
    return;
  }
  squares[i] = this.state.xIsNext ? 'X' : 'O';
  this.setState({
    squares: squares,
    xIsNext: !this.state.xIsNext,
  });
}
```

이제 기본적인 게임의 기능이 완성되었습니다!
