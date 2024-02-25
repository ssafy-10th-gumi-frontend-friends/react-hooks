# useRef

> 렌더링에 필요하지 않은 값을 참조할 수 있는 훅
> 

```jsx
const ref = useRef(initialValue)
```

→ 특정 DOM을 선택하는 것임 vanilla JS 의 document.querySelector()와 비슷하다

### 사용해야 하는 상황

컴포넌트 안에서 조회 및 수정할 수 있는 변수를 관리.

ex) 특정 엘리먼트의 크기를 가져와야 함, 스크롤바 위치를 가져와야함, 스크롤바 위치를 설정해야함, 포커스를 설정해줘야함,… 

ex) Video.js, JWPlayer 같은 HTML5 video 관련 라이브러리, D3, chart.js 같은 그래프 관련 라이브러리 등 외부 라이브러리 사용시에도 특정 DOM에 적용해야함..

### parameter

- initialValue : ref 객체의 current 프로퍼티의 초기값. 초기 렌더링 이후에는 무시된다.

### 반환값

- 단일 property를 가진 객체를 반환
- current : 처음에는 initialValue로 설정됨. [JSX노드에 ref 속성으로] [React에 ref 객체를] 전달을 하면, 리액트가 current 프로퍼티를 설정함.
- 다음 렌더링부터는 useRef가 같은 객체를 반환함.

### 사용방법

useRef() 를 사용하여 Ref 객체를 만들고, 이 객체를 우리가 선택하고 싶은 DOM에 ref 값으로 설정한다.

그러면 Ref 객체의 .current 값은 우리가 원하는 DOM을 가리킴.

### 1. 저장공간 (변수 관리)

useState랑은 뭐가다를까

- State가 변할때 마다 렌더링이 됨. State대신 Ref안에 값을 저장하면, 값을 아무리 변경해도 **컴포넌트가 재렌더링이 되지 않음.** 

- 컴포넌트가 아무리 렌더링이 되어도 Ref안에 저장되어 있는 값은 변화하지 않고 그대로 유지됨.

> State의 변화 → 렌더링 → 컴포넌트 내부 변수들 초기화
Ref의 변화 → 렌더링 X → 변수 값 유지됨
State의 변화 → 렌더링 → 그래도 Ref의 값은 유지됨
> 

### 2. DOM요소 접근

대표적으로는 input 요소를 클릭하지 않고 포커스를 주고싶을때 많이 사용됨

ex) 로그인 화면을 띄웠을 때 id를 넣는 Input을 굳이 클릭하지 않아도 자동적으로 포커스되게 해주기..!

### Ref의 값은 컴포넌트의 전 생애주기를 통해 유지된다.

```jsx
import {useEffect, useRef} from 'react'

export default function App() {
	const inputRef = useRef()

	useEffect(() => {
		console.log(inputRef)
		inputRef.current.focus();
	}, [])

const loginAlert = () = {
	alert(`환영합니다. ${inputRef.current.value}`)
	inputRef.current.focus();
}

return (
	<div>
		<input ref={inputRef} type='text' placeholder='id' />
		<button onClick={loginAlert}>Login</button>
	</div>
);
}

```

→ 페이지가 렌더링 될 때도 id input 창에 포커스가 되어 있고, 

alert의 확인을 누른 뒤에도 input으로 포커스가 됨.

- 참고 사이트
    
    [https://react-ko.dev/reference/react/useRef](https://react-ko.dev/reference/react/useRef)
    
    [https://itprogramming119.tistory.com/entry/React-useRef-사용법-및-예제](https://itprogramming119.tistory.com/entry/React-useRef-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%98%88%EC%A0%9C)
    
    [https://react.vlpt.us/basic/12-variable-with-useRef.html](https://react.vlpt.us/basic/12-variable-with-useRef.html)