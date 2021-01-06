# [Coding Test] 수 찾기

###### Update History

2020.01.07 - 최초 작성.



## 문제

N개의 정수 A[1], A[2], …, A[N]이 주어져 있을 때, 이 안에 X라는 정수가 존재하는지 알아내는 프로그램을 작성하시오.

##### 입력

첫째 줄에 자연수 N(1 ≤ N ≤ 100,000)이 주어진다. 다음 줄에는 N개의 정수 A[1], A[2], …, A[N]이 주어진다. 다음 줄에는 M(1 ≤ M ≤ 100,000)이 주어진다. 다음 줄에는 M개의 수들이 주어지는데, 이 수들이 A안에 존재하는지 알아내면 된다. 모든 정수의 범위는 -231 보다 크거나 같고 231보다 작다.

##### 출력

M개의 줄에 답을 출력한다. 존재하면 1을, 존재하지 않으면 0을 출력한다.

##### 입출력 예

```javascript
5
4 1 5 2 3
5
1 3 7 9 5
```

```javascript
1
1
0
0
1
```



## 풀이

이진탐색을 이용해 빠르게 탐색하는 것이 포인트인 문제.

```javascript
let fs = require('fs');
let input = fs.readFileSync('/dev/stdin').toString().split('\n');

const base = [...input[1].split(" ")].sort();
const compareList = input[3].split(" ");

compareList.map((c) => {
    let minIdx = 0;
    let maxIdx = base.length - 1;
    let found = false;
    while (minIdx <= maxIdx) {
      let midIdx = Math.floor((minIdx + maxIdx) / 2);
      if (c < base[midIdx]) maxIdx = midIdx - 1;
      else if (c > base[midIdx]) minIdx = midIdx + 1;
      else {
        found = true;
        break;
      }
    }
    found ? console.log(1) : console.log(0);
});
```



## 후기

백준 특성 상 input에 대한 처리를 까딱 잘못하면, 시간 초과로 실패하는 경우가 발생한다.

이진탐색 문제를 2문제 풀고 나니, 어떤 식으로 풀어가야하는지 약간의 감이 온다.