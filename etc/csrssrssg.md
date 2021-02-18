# CSR vs SSR & SSG

###### Update History

21.02.19 - 최초 작성.





## CSR(Client Side Rendering)

```html
...
<body>
  <div id="root"></div>
  <script src="app.js"></script>
</body>
...
```

일반적으로 body 안에 id가 root(혹은 app 등)인 div와 필요한 스크립트가 들어있는 js파일을 불러오는 script 태그, 이 두 태그가 있는 형태를 띄고 있다.

과거와 달리 컴퓨터 및 브라우저의 성능향상으로 인해,
서버로부터 화면 구성에 필요한 로직들과 사용되는 프레임워크나 라이브러리들의 소스코드들을 받아와, 브라우저에서 직접 화면을 동적으로 구성하는 것이 가능해졌다.

이러한 형태를 **CSR(Client Side Rendering)**이라 부른다.



### 장점

웹사이트 구성에 필요한 모든 파일과 데이터를 한번에 불러와 상황에 맞춰 동작하기 때문에,
사실상 하나의 페이지에서 마치 앱을 사용하는 것 같은 경험을 가져올 수 있다.

이처럼 하나의 페이지에서 모든 작업이 이루어지는 형태의 웹페이지를 **SPA(Single Page Application)**이라 부른다.



### 단점

#### 1. 낮은 초기 로딩 속도

아무래도 해당 웹사이트를 구성하기위해 필요한 모든 요소를 한번에 받아오기 때문에,
파일들의 총 용량이 클 수록 초기 로딩 속도가 느리다는 단점이 있다.



#### 2. 낮은 SEO(Search Engine Optimization)

검색엔진 사이트들은 내부의 서버에 등록된 웹사이트들을 탐색하며,
해당 사이트의 검색에 적합한 title, description, keywords, link 등을 크롤링한다.

특히 이 과정에서 HTML 파일을 유심히 들여다보게 되는데,
CSR처럼 body에 유의미한 정보가 적은 경우엔 페이지 분석에 어려움이 있다.





## SSR(Server Side Rendering)

Client로 부터 요청이 들어오면 Server에서 필요한 파일들과 Data를 종합해 html과 js파일을 생성 후 돌려준다.



### 장점

#### 1. 높은 초기 로딩 속도

요청한 페이지에 필요한 것들만 종합해 서버로부터 받음으로서,
CSR에 비해 상대적으로 로딩속도가 빠르다.



#### 2. 높은 SEO

서버로부터 완성된 HTML 파일을 받아오기 때문에, 검색엔진의 크롤러는 더욱 의미있는 데이터를 얻을 수 있다.



### 단점

#### 1. Blinking issue

초기 static 웹사이트에도 존재했던 페이지 이동 간에 생기는 깜빡임 현상이 발생한다.

따라서 CSR에 비해 좋지않은 UX를 제공할 수 있다.



#### 2. 서버 과부하

다수의 잦은 요청이 발생할 경우, 서버에 과부하가 걸릴 수 있다.



#### 3. 동적 반응에 필요한 기다림

렌더링은 끝났지만 스크립트 파일을 아직 받지 못했다면, 동적 행위에 대한 응답이 없는 경우가 있다.



## TTI & TTV

* TTI(Time To Interactable) : 상호작용이 가능해지는 시점까지 걸린 시간.
* TTV(Time To Viewable) : 보여지기까지 걸린 시간.

CSR과 SSR의 각 TTI, TTV 의 시점이 다르다.



### CSR에서 TTI, TTV

>  Request -> index.html -> script file request -> script files

두 번째 index.html을 불러왔을 때, body는 비어있는 상태이므로 아무것도 보여지지 않는다.

마지막 script files를 받고 나서야 비로소 보여지고, 상호작용 가능한 상태가 된다.

따라서 CSR의 경우, TTI 와 TTV 가 거의 동일하다.



### SSR에서 TTI, TTV

두 번째 index.htmll을 받은 시점부터 페이지에 화면이 보여진다.

그리고 상호작용이 가능한 시점은 마지막 script files를 받은 시점이다.

따라서 SSR의 경우, TTV < TTI 로 표현할 수 있다.

이처럼 TTV 와 TTI가 차이가 있기 때문에 SSR에서 상호작용에 대한 응답이 없는 경우가 발생할 수 있다.



## 개선점

CSR의 초기 로딩속도가 낮다는 단점을 해결하기 위해,
초기에 필요한 js 파일을 잘게 쪼개는 것을 방안으로 들 수 있다.

한 번에 많은 파일들을 로딩하는 것보다 필수적인 파일들을 우선적으로 로딩하도록
설계한다면 보다 개선할 수 있다.

SSR의 경우, 시간의 단차를 줄이기 위해 렌더링 최적화를 통해 시간을 단축시키거나 skeleton UI 등을 통해 UX를 챙기면서
체감되는 차이를 줄이는 방안도 존재한다.



## SSG(Static Site Generation)

정적 페이지를 생성해주는 라이브러리.

CSR의 경우, React와 더불어 사용되는 Gatsby가 있다.

React로 만든 CSR 페이지를 정적 페이지들로 미리 생성한 뒤 서버에 배포해둘 수 있다.

물론 동적 페이지 구성에 필요한 스크립트 파일도 함께 보관할 수 있다.



SSR의 경우, React + Next.js 조합이 있다.

Next.js의 경우 SSR을 지원하는 라이브러리인데, 최근 static generation 기능을 지원하고 있다.
또한 no pre-rendering / pre-rendering 도 지원하고 있어,
목적과 상황에 맞게 유연한 설계를 할 수 있다.