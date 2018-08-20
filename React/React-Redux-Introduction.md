# Redux - Introduction
> VELOPERT, 리액트를 다루는 기술, 길벗, 2018. <br>
[Official site] https://redux.js.org/

## Without Redux vs With Redux
![redux-introduction](images/redux-introduction.svg)
> https://voidsatisfaction.github.io/2016/09/27/redux-study2/


## Terms
- 스토어: 애플리케이션의 상태 값들을 내장하고 있습니다.
- 액션: 상태 변화를 일으킬 때 참조하는 객체입니다.
- 디스패치: 액션을 스토어에 전달하는 것을 의미합니다.
- 리듀서: 상태를 변화시키는 로직이 있는 함수입니다.
- 구독: 스토어 값이 필요한 컴포넌트는 스토어를 구독합니다.

## Three Principles
### 1. Single source of truth (스토어는 단 한 개)
  - The state of your whole application is stored in an object tree within a single store.

### 2. State is read-only (state는 읽기 전용)
  - The only way to change the state is to emit an action, an object describing what happened.

### 3. Changes are made with pure functions (변화는 순수 함수로 구성)
  - To specify how the state tree is transformed by actions, you write pure reducers.

## Example
리덕스는 리액트에서 사용하려고 만든 상태관리 라이브러리지만, **리액트에 의존하지는 않습니다.** <br>
즉, 리액트를 사용하지 않아도 리덕스를 사용할 수 있습니다.

### JavaScript
```js
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

const increment = (diff) => ({
  type: INCREMENT,
  diff: diff
});

const decrement = (diff) => ({
  type: DECREMENT,
  diff: diff
});

const initialState = {
  number: 1,
  foo: 'bar',
  baz: 'qux'
};

function counter(state = initialState, action) {
  switch(action.type) {
    case INCREMENT:
      return {
        ...state,
        number: state.number + action.diff
      };
    case DECREMENT:
      return {
        ...state,
        number: state.number - action.diff
      };
    default:
      return state;
  }
}

const { createStore } = Redux;
/*
  나중에 우리가 실제로 프로젝트에서 불러올 때는
  import { createStore } from 'redux';
  이렇게 불러옵니다.
*/

const store = createStore(counter);

const unsubscribe = store.subscribe(() => {
  console.log(store.getState());
});

store.dispatch(increment(1));
store.dispatch(decrement(5));
store.dispatch(increment(10));
```

### Console
```js
[object Object] {
  baz: "qux",
  foo: "bar",
  number: 2
}
[object Object] {
  baz: "qux",
  foo: "bar",
  number: -3
}
[object Object] {
  baz: "qux",
  foo: "bar",
  number: 7
}
```
