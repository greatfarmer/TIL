# LifeCycle API
![react-component-lifecycle](images/react-component-lifecycle.jpg)
> 그림 출처: [Understanding React component life-cycle](https://code.likeagirl.io/understanding-react-component-life-cycle-49bf4b8674de)

## 마운트
- ```constructor``` 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
- ```getDerivedStateFromProps``` props에 있는 값을 state에 동기화하는 메서드
- ```render``` 우리가 준비한 UI를 렌더링하는 메서드
- ```componentDidMount``` 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드

## 업데이트
- ```getDerivedStateFromProps``` 이 메서드는 마운트 과정에서도 호출하며, props가 바뀌어서 업데이트할 때도 호출
- ```shouldComponentUpdate``` 컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드 (여기에서 ```false```를 반환하면 아래 메서드들을 호출하지 않음)
- ```render``` 컴포넌트를 리렌더링
- ```getSnapshotBeforeUpdate``` 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드
- ```componentDidUpdate``` 컴포넌트의 업데이트 작업이 끝난 후 호출하는 메서드

## 언마운트
- ```componentWillUnmount``` 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드

## 기타
- ```componentDidCatch``` 에러가 발생한다면 잡아줄 수 있는 메서드

## 출처
> 1. VELOPERT, 리액트를 다루는 기술, 길벗, 2018.
> 2. https://velopert.com/reactjs-tutorials
