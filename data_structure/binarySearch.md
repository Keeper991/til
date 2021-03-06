# 이분탐색 (Binary Search)

###### Update History

2020.01.16 - 최초 작성.





## 본문

앞서 `입국심사` 문제를 통해서 이분탐색을 통한 문제해결을 경험했었다.

물론 스스로 해결하지 못했던 문제였고, 해설을 보고나서야 이분탐색이 문제에서 어떻게 사용되었는지를 알 수 있었다.

이분탐색 문제를 스스로 풀어보겠다는 도전으로, 새로운 문제에 시도했지만 역시나 실패했다.

그리고 역시나 해설을 찾아서 이해했는데 약간의 꼼수(?)가 눈에 보였다.



> **이분탐색 문제의 특징**
>
> 1. 제시되는 케이스의 범위가 엄청나게 넓다(1억이상).
>
> 2. 횟수가 주어진다.



그리고 **<u>'무엇을 찾을 것인가? 무엇으로 탐색할 것인가? 무엇을 비교할 것인가?'</u>** 라는 질문이 중요하다.



입국심사 문제의 경우, 
찾고자 하는 것은 모든 민원이 처리완료되는 최소 시간,
탐색에 쓰일 대상은 담당자들이 업무를 맡기 시작하는 시점들의 배열,
비교의 대상은 제시된 민원인의 수다.



이렇게 주어진 정보들을 각 질문에 맞게 매치한 뒤에 문제를 접한다면,
이분탐색 문제가 좀 더 쉽게 다가오지 않을까 싶다.