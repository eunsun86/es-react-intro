# Day 3. 플레이 히스토리 저장

게임을 진행하며, 모든 순간의 히스토리를 저장하고 또 언제든지 과거의 상태로 돌아갈 수 있도록 한번 만들어 봅시다. 이미 우리는 한번 플레이가 진행될때마다 각각의 네모칸에 들어가야할 값들을 새롭게 생성하여 보여주고 있습니다. 그래서 이 형식을 살짝만 바꾸어 주면, 쉽게 구현이 가능할 것 같습니다.

다음과 같은 형태로 히스토리를 저장해보도록 하겠습니다.

```javascript
history = [
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null
    ]
  },
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null
    ]
  },
  {
    squares: [
      null, null, null,
      null, null, null,
      null, null, null
    ]
  }
  // ...
];
```

그리고 우리는 최상위 컴포넌트인 `Game` 컴포넌트가 이런 히스토리들의 존재를 보여주도록 하겠습니다. 지난 시간에 `Square` 컴포넌트에서 `state`을 빼서 `Board` 컴포넌트로 올린 것과 비슷하게, 이번에는 `Board` 컴포넌트에서 `Game` 컴포넌트로 올려보겠습니다. 그래야 최상위 컴포넌트인 `Game` 컴포넌트가 필요한 정보들을 갖고 있을 수 있겠죠.

우선 `Game` 컴포넌트의 `constructor`를 다음과 같이 만들어 주세요.
```javascript
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      xIsNext: true,
    };
  }

  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}
```

그리고 `Board` 컴포넌트가 `squares`를 `props`로 받을 수 있게끔 수정하고 `onClick`도 `Game` 컴포넌트가 지정해주도록 바꾸어 보겠습니다. 그리고 각각의 `square`가 어떤 값인지를 받아서 알기 위해, 그 위치도 보내주겠습니다.

1. `Board` 컴포넌트에서 `constructor`를 지우세요.
2. `Board` 컴포넌트의 `renderSquare` 메소드에서 `this.state.squares[i]`를 `this.props.squares[i]`로 수정해주세요.
3. `Board` 컴포넌트의 `renderSquare` 메소드에서 `this.handleClick(i)`를 `this.props.onClick(i)`로 수정해주세요.

```javascript
class Board extends React.Component {
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

  renderSquare(i) {
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)}
      />
    );
  }

  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) {
      status = 'Winner: ' + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

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

`Game` 컴포넌트의 `render` 메소드는 가장 최근의 히스토리 엔트리를 보여주어야 하고, 누구의 순번인지를 보여주어야 합니다.

```javascript
render() {
  const history = this.state.history;
  const current = history[history.length - 1];
  const winner = calculateWinner(current.squares);

  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board
          squares={current.squares}
          onClick={(i) => this.handleClick(i)}
        />

      </div>
      <div className="game-info">
        <div>{status}</div>
        <ol>{/* TODO */}</ol>
      </div>
    </div>
  );
}
```

이제 `Game` 컴포넌트가 `status`를 보여주기 때문에, `<div className="status">{status}</div>` 부분과 `status`를 계산하는 부분은 `Board` 컴포넌트의 `render` 메소드에서 지워도 됩니다.

```javascript
render() {
  return (
    <div>
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
```

이번에는 `handleClick`을 `Board` 컴포넌트에서 `Game` 컴포넌트로 옮겨야 합니다.

그리고 `Game` 컴포넌트의 구조가 조금 다르기 때문에 약간의 수정이 필요합니다. `Game` 컴포넌트의 `handleClick` 메소드는 히스토리에 새로운 히스토리를 생성해서 넣어주어야 합니다.

```javascript
handleClick(i) {
  const history = this.state.history;
  const current = history[history.length - 1];
  const squares = current.squares.slice();
  if (calculateWinner(squares) || squares[i]) {
    return;
  }
  squares[i] = this.state.xIsNext ? 'X' : 'O';
  this.setState({
    history: history.concat([{
      squares: squares,
    }]),
    xIsNext: !this.state.xIsNext,
  });
}
```

이전의 게임 상태들을 보여주는 기능을 시도해봅시다. 첫 시간에 배웠듯이, React Element들은 단지 자바스크립트 객체일뿐이기 때문에 일반적인 자바스크립트 객체와 마찬가지로 여러가지를 할 수 있습니다. 여러개의 리스트를 보여주기 위해서는, React Element들로 이루어진 배열을 넘겨주면 됩니다. 가장 흔하게 쓰이는 방식은 `map`을 이용한 방식입니다.

`Game` 컴포넌트의 `render` 메소드에 다음과 같이 적용해보세요.
```javascript
render() {
  const history = this.state.history;
  const current = history[history.length - 1];
  const winner = calculateWinner(current.squares);

  const moves = history.map((step, move) => {
    const desc = move ?
      'Go to move #' + move :
      'Go to game start';

    return (
      <li>
        <button onClick={() => this.jumpTo(move)}>{desc}</button>
      </li>
    );
  });

  let status;

  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board
          squares={current.squares}
          onClick={(i) => this.handleClick(i)}
        />
      </div>
      <div className="game-info">
        <div>{status}</div>
        <ol>{moves}</ol>
      </div>
    </div>
  );
}
```

모든 히스토리의 단계마다 우리는 버튼이 담긴 `<li>`를 생성하고 클릭 핸들러를 보내줍니다. 위와 같이 수정하셨다면, 히스토리상에서 이전으로 돌아가는게 가능할 겁니다.

그리고 지금과 같은 상태에서 콘솔을 확인해보시면 아래와 같은 메시지를 보실겁니다.
```
Warning: Each child in an array or iterator should have a unique “key” prop. Check the render method of “Game”.
```

React Element에서 `key`라는 속성은 특별한 의미가 있습니다. React가 자식들을 업데이트할때 내부적으로 해당 레퍼런스를 기억해두고 조금 더 효율적으로 업데이트하기 위해서 존재합니다. 그래서 항상 여러개의 동일한 React Element들을 다이나믹하게 보여줘야하는 경우에는 `key` 값을 주는 것이 좋습니다.

이제 우리의 히스토리를 보여줄때, `key`값을 주도록 해보죠. `Game` 컴포넌트의 `render` 메소드를 다음과 같이 바꿔보세요.

```javascript
const moves = history.map((step, move) => {
  const desc = move ?
    'Go to move #' + move :
    'Go to game start';
  return (
    <li key={move}>
      <button onClick={() => this.jumpTo(move)}>{desc}</button>
    </li>
  );
});
```

버튼을 누르면 현재 에러가 발생할겁니다. `jumpTo`라는 메소드가 없기 때문이죠. 우선, `Game` 컴포넌트의 `state`에 우리가 현재 어떤 스텝을 보는 중인지를 판별할 새로운 키/밸류를 아래와 같이 추가해주세요.

```javascript
class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [{
        squares: Array(9).fill(null),
      }],
      stepNumber: 0,
      xIsNext: true,
    };
  }
```

다음으로 우리는 `jumpTo` 메소드를 `Game` 컴포넌트에 구현해야 합니다. 이때 중요한 것은, `xIsNext`도 함께 업데이트되어야 합니다.

```javascript
  handleClick(i) {
    // 이전과 동일
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0,
    });
  }

  render() {
    // 이전과 동일
  }
```

그리고 이제는 새로운 플레이가 추가될때마다 `stepNumber`를 업데이트해주어야 합니다.

```javascript
  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? 'X' : 'O';
    this.setState({
      history: history.concat([{
        squares: squares
      }]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext,
    });
  }
```

또 각각의 히스토리를 보여주기 위해, `Game` 컴포넌트의 `render` 메소드도 업데이트가 필요합니다.

```javascript
  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    // 이하 동일
```
