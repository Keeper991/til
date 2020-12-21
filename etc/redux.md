# Redux

###### Update History

2020.12.21 - 최초 작성.



## 등장이유

application에 있는 state를 효율적으로 관리하는 것을 돕기 위해 등장했다고 볼 수 있다.

관리되는 state는 reducer를 통해 수정될 수 있고, 이는 data가 여기저기서 변경될 위험성을 방지한다. 또한 일일이 update 전용 함수를 만들 필요없이, state의 변화를 감지해 함수를 호출하는 subscribe를 활용해볼 수 있다.



## 등장인물(?)

### Store

state의 저장소.

###### How to use

```javascript
import { createStore } from "redux";

const store = createStore(reducer);
```

###### methods

* getState() : 말그대로 현재 state 가져옴.
* dispatch(Object)
* subscribe(callback)



### Reducer

state에 대해 유일하게 실질적 조치를 취하는 콜백함수.

state를 초기화하고 변경한다.

###### How to use

```javascript
import { createStore } from "redux";

const reducer = (state = 0, action) => {
  return state;
}

const store = createStore(reducer);
```

* Param 1: 현재 state. default = undefined.
* Param 2: action. Reducer에 명령을 전달.
* Return: application에 주어질 state. state mutate가 아닌 state copy(new state)로 return.

```javascript
/* state에 +1 하고 싶다면, */

return ++state;			// X

return state + 1;		// O
```



### Dispatch

reducer의 2번째 인자인 action에 명령을 대입하고 호출하는 함수.

###### How to use

```java
import { createStore } from "redux";

const ADD = "ADD"

const reducer = (state = 0, action) => {
	switch(action.type) {
    case ADD:
      return state + 1;
    default:
      return state;
  }
}

const store = createStore(reducer);

store.dispatch({ type: ADD });
```

인자는 반드시 Object여야 하고, 'type'이라는 property를 정의해야한다.

(이 'type'을 통해, reducer는 어떻게 state를 다룰지에 대한 조치가 결정된다.)



### Subscribe

state의 변화를 감지해 콜백을 호출해주는 함수.

###### How to use

```javascript
import { createStore } from "redux";

const ADD = "ADD"

const reducer = (state = 0, action) => {
	switch(action.type) {
    case ADD:
      return state + 1;
    default:
      return state;
  }
}

const store = createStore(reducer);

store.subscribe(() => console.log(count.getState()));
store.dispatch({ type: ADD });

// dispatch로 state에 1이 추가되면,
// subscribe와 getState로 인해 현재 state를 출력.
```





## React에서의 Redux

위에서 언급한 등장인물들은 Javascript에서 사용된다.

Redux하면 React를 먼저 떠오르듯이 React와 Redux는 자주 사용되는 조합이다. 



React에서 사용하려면 `react-redux` 패키지를 설치해줘야한다.



```react
import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";
import { Provider } from "react-redux";
import store from "./store";	// state store

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```



### connect

Redux의 state와 component를 연결하기 위해선 connect가 필요하다.

```react
...
export default connect(mapStateToProps)(Home);
```

component를 export하는 부분에서 위와 같이 작성해주면, store에 있는 state에 해당 component가 접근할 수 있게 된다.



### mapStateToProps

getState에 해당하는 함수.

connect의 첫번째 인자로 들어가며, 반드시 object를 return 해야한다.

return한 object는 component에게 전달되는 state가 된다.
(store의 state를 return하지 않고, 임의의 객체를 return해도 그 임의의 객체가 component의 state가 된다.)



###### How to use

```react
import React, { useState } from "react";
import { connect } from "react-redux";

function Home({ toDos }) {
  console.log(toDos);	// state
}

 function mapStateToProps(state, ownProps) {
   return { toDos: state };
 }

 export default connect(mapStateToProps)(Home);
```





### mapDispatchToProps

store.dispatch() 역할의 함수.

method가 담긴 객체를 return해주면, props에 해당 method들이 담긴다.

###### How to use

```react
import React, { useState } from "react";
import { connect } from "react-redux";

function Home({ toDos, dispatch }) {
  console.log(toDos);	// state
  dispatch({ type: 'add': text: 'homework'});
}

function mapStateToProps(state, ownProps) {
  return { toDos: state };
}

function mapDispatchToProps(dispatch, ownProps) {
  return { dispatch };
}

export default connect(mapStateToProps, mapDispatchToProps)(Home);
```

첫번째 인자로 들어오는 dispatch가 store.dispatch()와 동일한 함수다.

다만 위와 같이 component에 직접 dispatch를 전달하는 형태보다는, 한 번 씌워서 전달하는 방법이 선호된다.

```react
...
function Home({ toDos, add }) {
  console.log(toDos);	// state
  add('homework');
}

...
function mapDispatchToProps(dispatch, ownProps) {
  return { add: (text) => dispatch({ type: 'add', text }) };
}
```

좀 더 캡슐화를 한다면, 다음과 같이 할 수 있다.

```react
// store.js
...
const ADD = 'ADD';

const addMenu = text => {
  return { type: ADD, text };
}

const actionCreators = {
  addToDo,
}

// Home.js
...
function mapDispatchToProps(dispatch, ownProps) {
  return { add: (text) => dispatch(actionCreators.addToDo(text)) };
}
```

(action 객체를 store 관련 파일로 떼어놓는 형식.)



### ownProps

mapStateToProps 와 mapDispatchToProps의 두 번째 인자.

사용되는 component의 props가 여기에 담긴다.

```react
// Todo.js

function menu({ text, onBtnClick }) {
  return (
  	<li>
    	{text} <button onClick={onBtnClick}>Del</button>
    </li>
  );
}

function mapDispatchToProps(dispatch, ownProps) {
  return {
    // Home에서 들어온 toDo의 id값을 불러옴.
    // store에 있는 actionCreators.deleteToDo(id)를 호출함.
    onBtnClick: () => dispatch(actionCreators.deleteToDo(ownProps.id));
  }
}

// Home.js
...
function Home({ toDos, add }) {
	return (
  	<ul>
    	{toDos.map(toDo => (
      	<ToDo {...toDo} key={toDo.id} />
      ))}
    </ul>
  );
}
```