# Redirect & Switch

> 2020.10.31 - 최초 작성.



## Redirect

말 그대로 Redirect 해주는 내장 컴포넌트.

`<Redirect from="*" to="/" />`  형태로 많이 사용된다.

from 속성으로 조건을 입력하고 to 속성으로 redirect 할 위치를 입력하면 된다.



## Switch

Route 다발을 Switch로 묶으면, 상단에서부터 조건에 부합하는 Route를 탐색한다.

조건에 맞는 Route를 찾을 경우, 해당 Route만 실행하고 이후의 Route에 대해선 탐색하지 않는다.



## 결론

Redirect를 Router안에서 사용할 경우, Switch와 함께 사용하는 것이 좋다.

특히 모든 url을 대상으로 한 Redirect의 경우, Switch를 사용하지 않으면 link를 눌러 해당 page로 전환되어도 다시 Redirect되는 현상이 발생한다.

따라서 Switch 안에 Redirect를 최하단에 배치함으로서, 이러한 현상을 해결할 수 있다.

```react
<Router>
	<Switch>
  	<Route path="/" exact component={Home} />
    <Route path="/content" exact component={Content} />
    <Route path="/search" exact component={search} />
		<Redirect from="*" to="/" />
  </Switch>
</Router>
```



