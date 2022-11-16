# React 복습/ 내용 정리

### Node.js
1. Webpack, Babel같은 도구들을 사용하기 위해 필요
* Babel : 자바스크립트 문법 확장해주는 도구 (구형 브라우저에서도 JS 문법이 제대로 실행될 수 있게 해줌) 
```
JSX ------> JS
    (Babel)
```
ㅁ. 설치방법
* 윈도우
```
https://nodejs.org/en/
```
* 맥
```
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
$ nvm install --lts
```

### useState 훅
```
import React, { useState } from 'react';

function Counter() {
  const [number, setNumber] = useState(0); // 기본값 0

  const onIncrease = () => {
    setNumber(number + 1);
  }

  const onDecrease = () => {
    setNumber(number - 1);
  }

  return (
    <div>
      <h1>{number}</h1>
      <button onClick={onIncrease}>+1</button>
      <button onClick={onDecrease}>-1</button>
    </div>
  );
}

export default Counter;
```
* useState가 없다면?
```
const numberState = useState(0);
const number = numberState[0];
const setNumber = numberState[1];
```

### useRef
1. 개념
JS에서 Dom을 선택해야 할 때, getElementById, querySelector 등의 Dom selector를 사용했음
-> 리액트에서는 ref를 사용.




### API 연동
1. requset 모듈, xml-js 모듈 설치
* 명령어
```
request 모듈 : yarn add request
xml-js 모듈 : yarn add xml-js
```
* 기타 모듈
```
xml2json : npm install xml2json
util : npm i react-util
```

2. axios 라이브러리 설치
: API를 호출하기 위해 사용하는 라이브러리
* 설치 명령어
```
cd 해당경로
yarn add axios
```
* axios 기본 명령어
```
Get : 데이터 조회
Post : 데이터 등록
Put : 데이터 수정
Delete : 데이터 제거
```

### 예제1. 클릭 및 카운트
1. App.js
```
import React from "react";
import "./styles.css";
import Counter from "./Counter";
import AutoCounter from "./AutoCounter";
import ManualCounter from "./ManualCounter";

export default function App() {
  return (
    <div className="App">
      <Counter />
      <hr />
      <AutoCounter />
      <hr />
      <ManualCounter />
    </div>
  );
}
```
*  counter.js
```
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);
  console.log(`랜더링... count: ${count}`);

  return (
    <>
      <p>{count}번 클릭하셨습니다.</p>
      <button onClick={() => setCount((count) => count + 1)}>클릭</button>
    </>
  );
}

export default Counter;

```
* AutoCounter.js
```
import React, { useState, useEffect } from "react";

function AutoCounter() {
  const [count, setCount] = useState(0);
  console.log(`랜더링... count: ${count}`);

  useEffect(() => {
    const intervalId = setInterval(() => setCount((count) => count + 1), 1000);
    return () => clearInterval(intervalId);
  }, []);

  return <p>자동 카운트: {count}</p>;
}

export default AutoCounter;
```
* ManualCounter.js
```
import React, { useState, useRef } from "react";

function ManualCounter() {
  const [count, setCount] = useState(0);
  const intervalId = useRef(null);
  console.log(`랜더링... count: ${count}`);

  const startCounter = () => {
    intervalId.current = setInterval(
      () => setCount((count) => count + 1),
      1000
    );
    console.log(`시작... intervalId: ${intervalId.current}`);
  };

  const stopCounter = () => {
    clearInterval(intervalId.current);
    console.log(`정지... intervalId: ${intervalId.current}`);
  };

  return (
    <>
      <p>자동 카운트: {count}</p>
      <button onClick={startCounter}>시작</button>
      <button onClick={stopCounter}>정지</button>
    </>
  );
}
export default ManualCounter;
```
