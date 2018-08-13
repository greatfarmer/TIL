# propTypes
> VELOPERT, 리액트를 다루는 기술, 길벗, 2018.

- array: 배열
- bool: 참, 거짓
- func: 함수
- number: 숫자
- object: 객체
- string: 문자열
- symbol: ES6 문법의 심벌 개체
- node: 렌더링할 수 있는 모든 것(숫자, 문자열, element 또는 이들로 구성된 배열)
- element: 리액트 요소
- instanceOf(MyClass): 특정 클래스의 인스턴스
- oneOf(['Male', 'Female']): 주어진 배열 요소 중 값 하나
- oneOfType([React.PropTypes.string, React.PropTypes.number]): 주어진 배열 안의 종류 중 하나
- arrayOf(React.PropTypes.number): 주어진 종류로 구성된 배열
- objectOf(React.PropTypes.number): 주어진 종류의 값을 가진 객체
- shape({name: React.PropTypes.string, age: React.PropTypes.number}): 주어진 스키마를 가진 객체
- any: 아무 종류
