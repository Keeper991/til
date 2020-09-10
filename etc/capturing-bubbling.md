# 이벤트 캡쳐링 & 버블링

이벤트는 **캡쳐링 > 타겟노드 이벤트 > 버블링** 순으로 발생한다.

그림으로 그린다면 U자로 나타낼 수 있을 것이다.



## 캡쳐링

target이 되는 이벤트가 발생했을 때, 해당 target의 최상위 노드부터 시작해 target까지 이벤트가 발생하는 것을 말한다. 

```html
<html>
  <body>
    <div>
      <p>
        <span>click</span>
      </p>
    </div>
  </body>
  <script>
    for(let elem of document.querySelectorAll('*')) {
      elem.addEventListener("click", e => alert(`캡쳐링: ${elem.tagName}`), true);
      elem.addEventListener("click", e => alert(`버블링: ${elem.tagName}`));
    }
  </script>
</html>
```

위의 코드로 작성된 페이지에서 'click'을 click했을 때,
**HTML > BODY > DIV > P > SPAN** 순으로 이벤트가 발생한다.

이벤트를 캡쳐링과정에서 발생시키고 싶다면
위와 같이 `addEventListener` 의 3번째 인자에 `true` 를 기입하면 된다.



## 버블링

캡쳐링과 반대로 타겟노드부터 최상위 노드까지,
**SPAN > P > DIV > BODY > HTML** 순으로 발생하는 것을 말한다.

`addEventListener` 의 3번째 인자에 `false` 를 기입하거나 생략하면 된다.



### event.stopPropagation()

이벤트를 등록한 노드를 기점으로 버블링을 멈추는 메소드.

타겟노드에 한해서만 이벤트를 발생하기 위해서 사용된다.

하지만 상위 요소에서 이벤트가 어떻게 쓰일지 확실치 않고, 추후에 버블링이 필요한 경우가 생길 수 있기 때문에 해당 메소드의 사용은 신중해야한다.

사용이 불가피할 경우 커스텀 이벤트를 통해 해결할 수 있다.





## 출처

* https://ko.javascript.info/bubbling-and-capturing#ref-709