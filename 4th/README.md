# 이벤트 처리하기

## html의 이벤트 처리 방식

```javascript

// button의 경우 아래와같이 처리
<button onClick={activateLasers}>
  Activate Lasers
</button>

// form의 경우 아래와 같이 "return false" 처리해주어 html 기본동작을 막아주어야함.
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>
```

## react 의 이벤트 처리 방식

```javascript
function Form() {
  // 아래와 같이 preventDefault를 통해 html 기본동작을 막아줄 수 있다.
  function handleSubmit(e) {
    e.preventDefault();
    console.log("You clicked submit.");
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

## class형 컴포넌트에서 eventlistener 처리하는 방법

1. custom method를 만든다.
2. this.customMethodName.bind(this) 해준다.
   - 화살표함수로 custom method를 만들었다면 bind해줄 필요가 없다.
3. this.customMethodName을 통해 JSX에 삽입이 가능하다.

<details>
    <summary>Function.prototype.bind() 란?<summary>

- bind는 javascrpt function의 기본 메소드이다.
- bind를 하게되면 function이 원래있던 객체 및 class의 this가 인식되게 됨.
- bind의 첫번째 인수는 this로 따를 대상함수이고, 두번째부터는 함수에 넣을 파라미터들이다.
- 예시는 다음과 같다.

```javascript
this.x = 9;
var module = {
  x: 81,
  getX: function () {
    return this.x;
  },
};

module.getX(); // 81

// retrieveX의 this는 module로 인식되지 않음 (bind해주지 않아서)
var retrieveX = module.getX;
retrieveX();
// 9 반환 - 함수가 전역 스코프에서 호출됐음

// module과 바인딩된 'this'가 있는 새로운 함수 생성
// 새 변수는 bind되었기 때문에 module을 this로 인식할 수 있음.
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```

</details>

# 조건부 렌더링

## 조건부 렌더링 방식

- if문 활용
- 논리연산자 (&&) 활용
- 삼항연산자 ( ? : ) 활용
- 엘리먼트 대신 null을 반환하면 엘리먼트가 렌더링되지 않음.

# 리스트와 Key

- 여러개 컴포넌트 렌더링 시 map 활용할 수 있다.
  - 단, map 활용 시 key를 추가해주어야함.

## Key

- react에서 어떤 항목을 변경, 추가, 삭제할 지 식별을 도와줌.
- key를 통해 map을 통해 생성된 여러개의 엘리먼트에 대해 각각의 고유성을 부여할 수 있게 됨.
- key값은 한 엘리먼트에 대한 고유한 값을 사용해야함.
  - 대부분은 id를 key로 사용
  - map의 index사용도 가능. 하지만 비추
    - 항목의 순서가 바뀔 경우 식별이 어려워진다.
  - 단, 전역 범위에 대한 고유한 값을 가질 필요는 없다.
    - map 안에서만 고유하면 됨.
