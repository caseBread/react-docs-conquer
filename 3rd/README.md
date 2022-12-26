# 리액트 컴포넌트

## 컴포넌트 종류는 두가지다:함수형, 클래스형

- 함수형 컴포넌트

  - props라는 파라미터를 받아 element를 반환함.

- 클래스형 컴포넌트
  - render method를 통해 element

## 컴포넌트가 렌더링되는 과정

1. root.render() 를 통해 `React.Component` 를 호출
2. `React.Component` 는 해당하는 return에 해당하는 값을 반환.
3. 리액트는 해당 반환 값을 효율적으로 DOM에 업데이트함.

## 컴포넌트 합성

- 컴포넌트는 자신의 반환값에 다른 컴포넌트를 이용할 수 있다.
- example

```javascript
function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
// App이라는 컴포넌트에서 Welcome이라는 컴포넌트를 호출
```

## 컴포넌트 추출

- 여러개의 element로 짜여진 컴포넌트를 관심사별 하나의 컴포넌트로 분리할 수 있다.

```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img
          className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">{props.author.name}</div>
      </div>
      <div className="Comment-text">{props.text}</div>
      <div className="Comment-date">{formatDate(props.date)}</div>
    </div>
  );
}

//위의 함수에서 Avatar부분을 하나의 컴포넌트로 추출 가능
function Avatar(props) {
  return (
    <img className="Avatar" src={props.user.avatarUrl} alt={props.user.name} />
  );
}
```

## (중요) component는 순수함수이다!

- 따라서, props의 값 변경은 X

# 리액트 State and Lifecycle

## class형 component에서 state 추가하기

- constructor를 통해 생성
  - React.Component상속을 통해 props멤버변수를 받은 후 state에 상태 추가하기
  - example

```javascript
constructor(props) {
    super(props);
    this.state = {date: new Date()};
}
// 이러면 class의 state 멤버변수가 업데이트됨
// this.state.date로 해당 변수 가져오기 가능.
// 한편, class형 component의 setState는 this.setState()로 동작함.
// example) state.number에 1 추가하는 방법 : setState((prev)=>({number : prev.number + 1}))

```

## class형 component의 메소드 알아보기 (fc의 hook에 해당)

- componentDidMount() : fc의 useEffect에 해당
- componentWillUnmount() : component가 unmount될 때 실행되는 메소드. 주로 eventListener, setInterval 등을 제거할 때 사용
- 다양함

## class형 component로 더하기빼기 만들어보기

```javascript
import React from "react";

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = { num: 1 };
  }

  handleNumberPlus = (e) => {
    console.log(e);
    this.setState({ num: this.state.num + 1 });
  };

  handleNumberMinus = () => {
    // state 직접수정은 X 알지?
    // setState를 통해 수정하자.
    this.setState((state, props) => ({
      // 파라미터는 state, props 순
      num: state.num - 1,
    }));
  };

  // hooks의 useEffect에 해당. render된 직후 발생
  componentDidMount() {}

  // 컴포넌트가 제거될 때 발생함. 주로 eventListener, setInterval 제거 등의 목적으로 사용
  componentWillUnmount() {}

  render() {
    return (
      <div>
        <p>hi im class component</p>
        <p>state number is {this.state.num}</p>
        <button onClick={this.handleNumberPlus}>+</button>
        <button onClick={this.handleNumberMinus}>-</button>
      </div>
    );
  }
}

export default App;
```
