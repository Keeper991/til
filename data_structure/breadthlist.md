# 노드 간 인접 리스트 쉽게 만드는 방법

###### Update History

2020.01.14 - 최초 작성.





## 설명

DFS나 BFS문제를 풀 때, 인접 행렬이나 리스트가 필요한 경우가 많다.

둘 다 장단점이 있지만 일반적으로 행렬에 비해 리스트가 시간복잡도, 공간복잡도 면에서 평균적인 성능을 보인다.

(행렬의 경우 n^2의 복잡도를 가진다..)



간선 배열이 주어질 경우 빠르게 리스트를 만드는 코드.

```javascript
const list = edge.reduce((L, [from, to]) => {
  L[from] = (L[from] || []).concat(to);
  L[to] = (L[to] || []).concat(from);
  return L;
}, {});
```