# [Coding Test] 조이스틱

###### Update History

2020.01.14 - 최초 작성.





## 문제

조이스틱으로 알파벳 이름을 완성하세요. 맨 처음엔 A로만 이루어져 있습니다.
ex) 완성해야 하는 이름이 세 글자면 AAA, 네 글자면 AAAA

조이스틱을 각 방향으로 움직이면 아래와 같습니다.

```
▲ - 다음 알파벳
▼ - 이전 알파벳 (A에서 아래쪽으로 이동하면 Z로)
◀ - 커서를 왼쪽으로 이동 (첫 번째 위치에서 왼쪽으로 이동하면 마지막 문자에 커서)
▶ - 커서를 오른쪽으로 이동
```

예를 들어 아래의 방법으로 JAZ를 만들 수 있습니다.

```
- 첫 번째 위치에서 조이스틱을 위로 9번 조작하여 J를 완성합니다.
- 조이스틱을 왼쪽으로 1번 조작하여 커서를 마지막 문자 위치로 이동시킵니다.
- 마지막 위치에서 조이스틱을 아래로 1번 조작하여 Z를 완성합니다.
따라서 11번 이동시켜 "JAZ"를 만들 수 있고, 이때가 최소 이동입니다.
```

만들고자 하는 이름 name이 매개변수로 주어질 때, 이름에 대해 조이스틱 조작 횟수의 최솟값을 return 하도록 solution 함수를 만드세요.

##### 제한 사항

- name은 알파벳 대문자로만 이루어져 있습니다.
- name의 길이는 1 이상 20 이하입니다.

##### 입출력 예

| name   | return |
| ------ | ------ |
| JEROEN | 56     |
| JAN    | 23     |



## 풀이

순간순간에 최적의 선택을 하는 것으로 전체적으로 최적의 결과를 도출하는 Greedy 알고리즘 문제다.

역회전이 가능하다는 조건때문에 조금 난이도가 있어보였던 문제다.

각 알파벳을 맞추는 것까진 쉬웠지만, 알파벳 간에 움직이는 횟수의 최솟값을 구하는 부분에서 애를 많이 먹었다.

돌아가는 경로의 경우, 어느 지점에서 돌아가야하는지 그 지점을 찾는 것이 힘들었다.

여러 테스트 케이스들을 만들면서 시험한 결과, 연속된 A의 길이가 최대인 지점이 터닝 포인트였다.

다른 사람의 풀이에선 0을 만나는 모든 순간들을 터닝 포인트로 잡고, 반복을 통해 최소값을 구하는 방식이었다.

풀고 나니, 후자의 방식이 좀 더 빠르고 나아 보였다..



```javascript
function solution(name) {
    // 1. 각 알파벳을 맞추기 위해 필요한 조이스틱 횟수 구하기.
  	// nums: 각 알파벳에 필요한 조이스틱 횟수. 0이면 A.
    let nums = [];  
  	var answer = 0;
    
    for(let i = 0; i < name.length; i++) {
        nums.push(name.charCodeAt(i) - 'A'.charCodeAt(0));
    }
    
  	// 절반이 최대값이므로, 절반 이상값은 총 알파벳갯수에서 마이너스.
    nums = nums.map(n => n > 13 ? 26 - n : n);  
    answer = nums.reduce((a, c) => a += c, 0);  // 일단 answer에 저장.
    
    // 2. 문자 간 이동에 필요한 조이스틱 횟수 구하기.
    // 연속된 A(값으로 0)가 가장 긴 구간(길이가 중복일 경우 첫 번째)을 기준으로 돌아가는 경로(여기서 left)를 구할 예정.
    // 그리고 돌아가지 않는 일반적인 경로(여기서 right)도 함께 구해서, 둘을 비교해 작은 값을 answer에 더함.
    
  
  	// zlen: nums에서 0의 길이.
  	// zIdx: numsd에서 0이 있는 index(0이 연속으로 나오는 구간의 경우, 첫 0의 index).
  	// zCnt: 반복문 돌면서 연속된 0의 길이를 누산할 버퍼.
    const zlen = [];    
    const zIdx = [];    
    let zCnt = 0;       
    for(let i = nums.indexOf(0); i < nums.length; i++) {
        if(nums[i] !== 0) {
            if(zCnt !== 0) {    // 0이 아니면 무조건 zlen에 집어넣는 걸 방지.
                zlen.push(zCnt);
                zCnt = 0;
            }
        } else {
            if(zCnt === 0) zIdx.push(i);    // 첫 0을 발견하면 zIdx에 기록.
            zCnt++;
        }
    }
    
    // A가 하나도 없는 경우, 처음부터 끝까지 한 방향으로 움직이는 횟수를 answer에 더함.
    if(zIdx.length === 0 || zlen.length === 0)
        return answer + name.length - 1;
    
    
    // maxZlen: 0이 연속으로 나온 구간들 중 최대값.
    // maxZIdx: maxZlen이 시작되는 Idx.
    const maxZlen = Math.max(...zlen);  
    const maxZIdx = zIdx[zlen.indexOf(maxZlen)];    
    
    // left: 돌아가는 경로.
    // right: 한 방향 경로.
    let left = 0;
    let right = 0;
    
    // totalNonZero: 0이 아닌 값의 개수. right만 진행할 경우, 끝까지 갈 필요가 없을 수 있음(마지막에 ~A로 끝나는 경우).
    // count: 누산된 0이 아닌 값의 개수.
    const totalNonZero = nums.filter(n => n > 0).length;
    let count = 0;
    
    // right 먼저 구함.
    for(let i = 0; i < nums.length; i++) {
        if(nums[i] !== 0) count++;
        if(count === totalNonZero) {
            right = i;
            break;
        }
    }
    // left 구함.
    // left = (maxZIdx까지 갔다가 처음으로 돌아오는 횟수) + (우측 끝에서 maxZlen이 끝나는 곳까지 돌아가는 경우)
    left = ((maxZIdx - 1) * 2) + (nums.length - 1 - (maxZIdx + maxZlen - 1));
    
    answer += Math.min(left, right);
    
    return answer;
}
```

