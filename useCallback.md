# useCallback

- useMemo 함수와 더불어 성능 최적화에 사용됨
- 훅을 사용하면서 컴포넌트가 렌더링될 때마다 함수를 생성해서 자식 컴포넌트의 속성으로 넘겨주게 된다.

```jsx
function App() {
	const [name, setName] = useState('');
	const onSave = () => {}

	return (
		<div className='App'>
			<input type='text' value={name} onChange={(e) => setName(e.target.value)}
/>
	<Profile onSave={onSave} />
	</div>
);
}

```

name이 변경되어 렌더링이 될 때,

onSave 함수가 새로 만들어지고,

Profile 속성으로 **새로운 함수**를 넣어주고 있다. 

이때 Profile 컴포넌트에서 React.memo를 사용해도,

이전 onSave와 이후 onSave가 매번 다르게 되어 매번 렌더링이 된다. 

이때 **Profile 재렌더링을 방지하기 위해 useCallback을 사용**한다. 

## 메모이제이션

첫번째 인자로 넘어온 함수를, 두번째 인자로 넘어온 배열 내의 값이 변경될 때까지 저장해놓고 재사용가능

```jsx
const memoizedCallback = useCallback(함수, 배열)
```

예를들어, 어떤 함수 컴포넌트 안에 함수가 선언이 되어있다면,

해당 컴포넌트가 렌더링될 때마다 새로운 함수가 생성이 됨.

```jsx
const add = () => x + y
```

useCallback을 사용하면 컴포넌트가 렌더링되더라도 그 함수의 의존값이 바뀌지 않는 이상 기존 함수를 계속해서 반환함.

```jsx
const add = useCallback(() => x+y, [x, y])
```

### 한계

- 컴포넌트가 렌더링될때마다 함수 새로 선언하는게 성능상 큰 문제가 되진 않음.
- 그러면 어떻게 사용해야 의미있게 쓴다고 볼수있나?

### 자바스크립트 함수 동등성

- 함수도 객체로 취급이 되기 때문에 메모리 주소에 의한 참조 비교가 일어나 동일한 형태의 함수도 동등연산자에서 false를 반환
- 이러한 특성때문에 함수컴포넌트 내에서 어떤 함수를 다른 함수의 인자로 넘기거나,
- 자식 컴포넌트의 prop으로 넘길 때 예상치 못한 성능 문제로 이어질 수 있다.

### 의존배열로 함수를 넘길때

```jsx
function Profile({ userId }) {
	const [user, fetchUser] = useState(null);
	
	const fetchUser = () => {
		fetch(api uri)
		.then((response) => response.json())
		.then(({user}) => user)

	useEffect(() => {
		fetchUser().then((user) => setUser(user))
	}, [fetchUser])

...
```

위의 상황에서 fetchUser 함수가 변경될 때만 api 요청 함수가 호출됨

그런데, fetchUser는 함수이기 때문에, userId 값이 바뀌든 말든 컴포넌트가 랜더링될 때마다 새로운 참조값으로 변경이됨.

그러면 useEffect() 함수가 호출되어 user 상태값이 바뀌고, 그러면 다시 렌더링되고 다시 useEffect() 함수가 호출되는 악순환이 일어남.

→ useCallback을 이용하면 

```jsx
function Profile({ userId }) {
	const [user, fetchUser] = useState(null);
	
	const fetchUser = useCallback(
		() => {
			fetch(api uri)
				.then((response) => response.json())
				.then(({user}) => user),
			[userId]
		);

	useEffect(() => {
		fetchUser().then((user) => setUser(user))
	}, [fetchUser])

...
```

userId 값이 변경되지 않는 한 재호출되지 않음.

## 주의사항

- 실질적 성능 이점이 어느정도인지 직접 측정하여 사용하기를 바람

- 참고한 사이트
    
    [https://k-developer.gitbook.io/dev/react/react-hook/usecallback](https://k-developer.gitbook.io/dev/react/react-hook/usecallback)
    
    [https://www.daleseo.com/react-hooks-use-callback/](https://www.daleseo.com/react-hooks-use-callback/)