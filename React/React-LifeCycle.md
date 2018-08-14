# LifeCycle API
> https://velopert.com/reactjs-tutorials

- ```constructor``` 컴포넌트가 새로 만들어 질 때 호출
- ```getDerivedStateFromProps``` Props의 값을 State와 동기화 시키고 싶을 때 사용
- ```shouldComponentUpdate``` 컴포넌트가 업데이트를 할 지 말지 정하는 것
- ```getSnapshotBeforeUpdate``` 실제로 브라우저 반영 되기 바로 직전의 값을 확인
- ```componentDidMount``` 마운트가 끝났을 때
- ```componentDidUpdate``` 업데이트가 끝났을 때
- ```componentWillUnmount``` 언마운트 되었을 때
- ```componentDidCatch``` 에러가 발생한다면 잡아줄 수 있는 함수
