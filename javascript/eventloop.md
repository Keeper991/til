# Event Loop

###### Update History

2020.02.18 - 최초 작성.





## 개념

JS의 Event Loop를 이해하기 위해선, JS가 구동되는데에 필요한 몇 가지 개념들이 필요하다.

1. Call Stack
2. Task Queue(또는 Callback Queue)
3. Web API
4. Asynchronous Process



![Event Loop Image](https://miro.medium.com/max/2048/1*4lHHyfEhVB0LnQ3HlhSs8g.png)

###### Call Stack

여느 프로그래밍 언어처럼 JS도 Call Stack이 존재한다.

함수가 실행되면 Call Stack에 쌓고, 함수가 끝나면 Call Stack에서 지운다.

JS에서 함수를 실행하는 방식을 보통 "Run to Completion"이라고 부르는데,
이는 하나의 함수가 실행되면 끝날 때까지 다른 작업이 중간에 끼어들지 못한다는 의미이다.

전에 C++을 배울 때 멀티스레드를 공부한 적 있었는데
조금만 디버깅하려하면 다른 Thread가 끼어들어 (Thread Switching이 빈번히 일어나) 어려워했던 기억이 있다.



###### Task Queue(또는 Callback Queue)

비동기처리 후 실행될 Callback 함수(Task)들이 담긴 큐.

`setTimeout(cb, ms)` 함수는 알다시피 ms 만큼 뒤에 cb를 호출하는 함수이다.

정확히 ms 뒤에 동작하는 것은 아니지만,
(<u>"최소한 ms 정도 있다가 실행해라"</u> 는 의미에 가깝다.)
얼추 ms 만큼 뒤에 Task Queue에 Callback 함수를 Enqueue한다.



###### Web API

> 웹 앱과 웹 컨텐츠가 기기의 하드웨어에 접근하고,
> 기기의 데이터 저장소에 접근할 수 있도록 해주는 기기호환과 접근 API의 모음을 나타내는 말.
>
> *(출처: https://developer.mozilla.org/ko/docs/WebAPI)*

스마트폰으로 치면, GPS나 자이로센서 등을 웹에서 인지할 수 있게 돕는 API들의 집합이다.

setTimeout 함수는 하드웨어의 타이머를 이용한 함수이기 때문에(?),
엄밀히 JS 내장 함수가 아니라 Web API에 속한다.

또다른 자주사용되는 API로,
서버에게 비동기로 Request를 날리고 Response를 받는 `XMLHttpRequest` 가 있다.



###### Asynchronous Process

JS에서 비동기 처리란,
"특정 코드의 실행이 완료될 때까지 기다리지 않고, 다음 코드를 먼저 수행하는 특성"을 의미한다.

비동기 함수가 실행되면 해당 함수를 일단 넘기고 다음 함수를 실행한다는 것이다.

예시로, `setTimeout` 은 Callback 함수를 실행하고 완료될 때까지 기다리지 않고,
다음 코드를 바로 실행한다.

많은 사람들이 비동기와 논블록을 헷갈려한다.

논블록과 비동기의 차이를 간단히 정리하면,
작업의 완료를 직접 확인해야하는지, 혹은 알려주는지의 차이라고 본다.



## 흐름

```javascript
function a () {
  console.log('function a start');
  setTimeOut(function() {
    console.log('setTimeOut callback!');
  }, 0);
	console.log('function a end');
}

a();

// function a start
// function a end
// setTimeout callback!
```

1. `function a` 가 CallStack에 추가된다.
2. `console.log('function a start')` 를 실행한다.
3. `setTimeOut` 을 실행한다.
4. `function () { console.log('setTimeOut callback!'); }` 이 Task Queue에 추가된다.
5. `console.log('function a end')` 를 실행한다.
6. `function a` 를 CallStack에서 제거한다.
7. Event Loop 가 Task Queue에서 `function () { console.log('setTimeOut callback!'); }` 를 뽑아내 CallStack에 추가한다.
8. `function () { console.log('setTimeOut callback!'); }` 를 실행한다.
9. `function () { console.log('setTimeOut callback!'); }` 를 CallStack에서 제거한다.