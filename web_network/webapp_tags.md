# Web App과 관련된 몇 가지 tags

###### Update History

2020.01.12 - 최초작성





## 설명

Web App 지원을 위해 관련된 몇 가지 tag들이 있다.

```html
< link rel="apple-touch-icon" href="/apple-touch-icon.png"/>
< link rel="apple-touch-startup-image" href="/startup.png">
< meta name="apple-mobile-web-app-capable" content="yes" />
< meta name="apple-mobile-web-app-status-bar-style" content="black" />
```



## apple-touch-icon

아이폰에서 웹사이트를 추가하게 되면 아이콘이 생기는데, 해당 아이콘의 이미지를 설정하는 것.
favicon의 아이폰 버전이라고 생각하면 된다.

57x57, 72x72, 114x114 사이즈의 png를 사용하는데, 114x114 이미지를 설정해두면 알아서 resize해서 사용한다.



기본적으로 해당 이미지에다가 둥근 모서리 + 반원형태의 하이라이트를 추가해주는데,
원치 않을 경우 rel 속성값을 `apple-touch-icon-precomposed` 라고 지정해주면 된다.

```html
<link rel=”apple-touch-icon-precomposed” href=”/apple-touch-icon-precomposed.png”/>
```



이렇게 지정한 아이콘 이미지는 안드로이드의 Add to Home Screen 기능에서도 지원된다.
사이즈는 48x48.



## apple-touch-startup-image

화면 로딩 시 스타트업 이미지를 지정할 수 있다.

아이폰 기본 앱에 들어있는 Default.png와 같은 역할이라고 한다.



## apple-mobile-web-app-capable

Web App으로 선언해 브라우저의 UI를 안 보이도록 할 수 있다. (전체화면 표시)



## apple-mobile-web-app-status-bar-style

상태바 색상을 지정할 수 있다. 웹앱의 색상 컨셉과 상태바의 색상이 잘 맞지 않을 경우에 사용한다.