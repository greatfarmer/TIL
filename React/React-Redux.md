# Redux
> VELOPERT, 리액트를 다루는 기술, 길벗, 2018. <br>
[Official site] https://redux.js.org/ <br>
[한글번역] https://deminoth.github.io/redux/

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

## Use comfortable
- [Immutable.js](https://facebook.github.io/immutable-js/)
- [Ducks](https://github.com/erikras/ducks-modular-redux)
- [redux-actions](https://redux-actions.js.org/)
