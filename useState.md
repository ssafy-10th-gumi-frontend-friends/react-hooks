# useState

### useState란?

> 컴포넌트에 상태 변수를 추가할 수 있도록 해주는 훅 함수.
> 
- 반환값 : 2개의 원소를 갖는 배열
    - 첫번째 원소 : 상태 값
    - 두번째 원소 : 상태 값을 변경할 때 사용되는 setter 함수
    
    → **구조분해할당**으로 선언함
    

```jsx
const [state, setState] = useState(initialState);
```



### 상태값 변경시 리렌더링 발생

setter 함수를 호출하면 상태값을 변경할 수 있음.

→ 리렌더링 진행

### 상태값 변경은 비동기적으로 동작

바로 값으로 상태 업데이트가 되는 것이 아니라, 대기열에 들어감. 

setter 함수 호출 후 바로 state값 참조시 바뀌기 전의 state가 참조됨.

### 업데이터 함수로 상태를 변경해야 하는 경우

setter 함수의 인자에는 값을 넣을 수도 있지만, **업데이터 함수**를 넣을 수도 있다.

> 업데이터 함수란?
인자에 prev 값이 넘어오면서 next state 값이 반환되도록 정의된 함수.

언제 사용? 
바로 이전 값을 참조해야 하는 경우 (아래 예시로 이해하자)
> 

```jsx
const [number, setNumber] = useState(0);

useEffect(() => {
  setNumber(prev => prev + 1);
  setNumber(prev => prev + 1);
  setNumber(prev => prev + 1);
  setNumber(prev => prev + 1);
  setNumber(prev => prev + 1);
}, []);
```

위 코드를 실행하면 number는 5로 바뀌어 있음. 

업데이터 함수 자체도 인자에 이전 상태값을 받아올 수 있지만,

업데이터 **함수들끼리도** 이전에 호출된 업데이터 함수에서 반환된 상태값을 이전 상태값으로 참조할수 있다!  

즉, 이전 값이 반드시 필요한 상태 업데이트 경우에 업데이터 함수 사용하면 됨.

### state 값이 바뀌고 다시 렌더링되는데까지

약 1ms ~ 2ms 내외 변경된다 함.

### 짧은 시간 안에 setter 함수 연속 호출시 렌더링은 1번만

리액트의 **배치 상태 업데이트**때문이다**.**

> 배치 상태 업데이트란?
짧은 시간에 연속 호출 시, 호출된 값 또는 업데이터 함수들을 큐에 집어 넣고 대기.
> 
> 
> 큐에 집어 넣은 것들은 순차적으로 일괄적으로 호출(적용)됨.
> 

- 참고사이트
    
    [https://react.dev/learn#using-hooks](https://react.dev/learn#using-hooks)
    
    [https://funveloper.tistory.com/192](https://funveloper.tistory.com/192)