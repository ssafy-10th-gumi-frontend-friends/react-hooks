# useMemo

이전 값을 기억해 성능을 최적화하는 용도로 사용한다.

```jsx
export default function Memo = ({ v1, v2}) => {

	// useMemo 훅
	const value = useMemo(() => {
		return v1 + v2;
	}, [v1])
	

	return <div>{value}</div>
};

```

useMemo **첫번째 매개변수로 함수**를 입력하고, **두번째 매개변수로 배열**을 입력한다.

첫번째 매개변수 return 값을 기억하고 있고 그 값을 value 에 할당한다.

두번째 매개변수 배열의 값이 변경되지 않으면 이전에 반환했던 값을 재사용. 만약 배열의 값이 바뀌었다면, useMemo 첫번째 매개변수인 함수를 재실행하여 그 return 값을 기억한다.

결론적으로 위 코드는 v1 값이 바뀌면 1번 함수가 실행되고 v1이 안 바뀌면 **이전 return 값을 재사용한다.** 

## 메모이제이션

기존 수행한 연산의 결과값을 어딘가에 저장해두고 동일한 입력이 들어오면 재활용하는 프로그래밍기법. 중복연산을 피할 수 있기 때문에 메모리를 조금 더 쓰더라도 애플리케이션 성능 최적화 가능

## 함수형 컴포넌트에 메모이제이션 적용

- 렌더링이 일어날 때마다, 인자로 넘어오는 값이 항상 바뀌는 게 아니라면 굳이 함수를 계속 호출할 필요가 없음.

### 예시 - useMemo 없이 매번 함수 호출

```jsx
import React from "react";

function SortedWords({ words }) {
  const sortWords = () => {
    console.log("sortWords");
    delay(500);
    return words.sort();
  };

  const sortedWords = sortWords(); // 함수가 매번 동작함

  return (
    <>
      <h2>Sorted Words</h2>
      <ul>
        {sortedWords.map((word, idx) => (
          <li key={idx}>{word}</li>
        ))}
      </ul>
    </>
  );
}

function delay(ms) {
  const now = new Date().getTime();
  while (new Date().getTime() < now + ms) {}
}
```

### → useMemo 사용시

```jsx
import React, { useMemo } from "react";

function SortedWords({ words }) {
  const sortWords = () => {
    console.log("sortWords");
    delay(500);
    return words.sort();
  };

  const sortedWords = useMemo(sortWords, [words]); // FAST

  return; /* 생략 */
}
```

words 프롭이 달라졌을 때에만 sortWords 함수가 호출이 되고, words prop이 동일할 때에는 최초 호출 결과가 계속해서 재사용된다.

## 주의사항

- 컴포넌트 복잡도 올라감 (코드 읽기 어려워짐, 유지보수성 떨어짐)
- useMemo가 적용된 레퍼런스는 재활용을 위해 가비지 컬렉션에서 제외되므로 메모리를 더 쓰게 됨.
- 실제 웹 프로젝트에서 useMemo 훅을 쓸 일이 그렇게 많지는 않다.
- 오래 걸리는 로직이 있다고 해도 useEffect로 비동기 처리하는 방법이 있음.
- 무분별하게 사용하지 말자!

- 참고한 사이트
    
    [https://k-developer.gitbook.io/dev/react/react-hook/usememo](https://k-developer.gitbook.io/dev/react/react-hook/usememo)
    
    [https://www.daleseo.com/react-hooks-use-memo/](https://www.daleseo.com/react-hooks-use-memo/)