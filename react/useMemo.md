# useMemo

###### Update History

2021.03.17 - 최초 작성.





## useMemo란?

알고리즘 유형 중에 Dynamic Programming을 보면,
이미 계산된 결과값을 재사용해야하는 경우가 발생한다. 이 때, 결과값을 저장해두는 방식을 memoization이라고 하는데, useMemo 역시 값의 재사용을 통해 불필요한 자원소모를 막을 수 있는 hook이다.





## How to use

```react
import { useMemo } from "react";

const SortedWords = ({ words }) => {
	const sortWords = () => {
		console.log(`call. words:${words}`);
		return words.sort();
	};

  // useMemo 사용.
	const sortedWords = useMemo(sortWords, [words]);

	return (
		<>
			<ul>
				{sortedWords.map((word, idx) => (
					<li key={idx}>{word}</li>
				))}
			</ul>
		</>
	);
};

const Count = () => {
	const [value, setValue] = useState("");
	const [words, setWords] = useState([]);

	const handleButton = () => {
		setWords([...words, value]);
		setValue("");
	};

	return (
		<>
			<input
				onChange={({ target: { value } }) => setValue(value)}
				value={value}
			></input>
			<button onClick={handleButton}>add</button>
			<SortedWords words={words} />
		</>
	);
};

export default Count;
```



10번 라인에서 useMemo를 사용하고 있다.

1번째 인자로 값이 변동될 경우 호출할 함수,
2번째 인자로 변동 대상이 되는 값(useEffect의 2번째 인자와 동일).



useMemo 대신,
`const sortedWords = sortWords();`
로 수정한다면 input의 value값이 변할 때마다 `sortWords` 함수가 호출되고
출력창에는 `call. words:${words}` 가 계속 찍힐 것이다.



## 최적화, 하지만 단점도..

대부분의 성능 최적화가 그렇듯, useMemo 또한 최적화라는 이점이 존재하지만 단점도 존재한다.

남용하다보면 코드 복잡성 및 유지보수 난이도가 증가하고, 적용된 reference는 가비지 컬렉션에서 제외되기 때문에 메모리를 더욱 사용하게 된다.



실제로 코딩하다보면 useMemo를 사용해야할 순간이 많지 않다고 한다.
오래 걸리는 로직이 있는 것 자체가 일반적이지 않은 일이거니와, 있다해도 useEffect 등으로 비동기처리하는 방안을 먼저 고려하기 때문이라고 한다.

게다가 브라우저에서 React의 실행속도가 결코 느리지 않다보니 웬만한 Re-rendering에도 심각한 성능 이슈가 발생하는 경우는 의외로 적다고 한다.

따라서, 무분별한 useMemo의 사용은 지양해야되며 정말 필요한 경우에 사용하도록 하자.



## 출처

* https://www.daleseo.com/react-hooks-use-memo/
* https://react.vlpt.us/basic/17-useMemo.html