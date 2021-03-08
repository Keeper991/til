# Wheel Event (feat. Full Page Scroll, animation)

###### Update History

2021.03.09 - 최초 작성.





## Wheel Event & Scroll Event

기존의 mousewheel event는 비표준으로 분류되었고,
wheel event로 대체되었다.

Wheel Event 와 Scroll Event는 엄밀히 구분된다.

브라우저 창 우측에 있는 스크롤을 Drag&Drop을 통해 움직인다면,
Scroll Event가 발생하지만, Wheel Event는 발생하지 않는다.

Wheel Event는 말그대로 Wheel을 사용했을 때 발생하는 이벤트이기 때문이다.

보편적으로 Wheel => Scroll로 인식하고 있고 그렇게 사용해왔기 때문에
약간의 혼동이 올 수 있다.



## Wheel Event를 활용한 Full Page Scroll 구현

Wheel Event를 이용해 간단하게 Full Page Scroll 을 구현하는 방법으로,
'스크롤을 손댈 것인지', '컨텐츠의 좌표를 손댈 것인지' 등 다양하게 존재한다.

이번엔 전자의 방법으로 구현해보았다.

(Pagination은 생략하였다..)

```html
<body>
  <section class="page">page1</section>
  <section class="page">page2</section>
  <section class="page">page3</section>
</body>
```

```css
body {
  font-family: sans-serif;
	min-height: 100vh;
	display: flex;
	justify-content: center;
	align-items: center;
	margin: 0;
	padding: 0;
}

.page {
  min-height: 100vh;
	display: flex;
	justify-content: center;
	align-items: center;
}
```

```javascript
const MIN_PAGE = 0;	// 페이지 시작점.
const MAX_PAGE = 2;	// 페이지 끝지점.  총 3개의 page.
let isWheeling = false;	// wheel 진행 여부 체크.
let timer;	// wheel event 처리를 한번만 하기 위한 타이머.
let nowPage = MIN_PAGE;

window.addEventListener("wheel", (e) => {
  	e.preventDefault();		// scroll event 차단.
	  clearTimeout(timer);	// 타이머 초기화.
		const delta = e.wheelDelta;		// wheel 방향 확인을 위한 값.
		if (!isWheeling && delta > 0 && nowPage < 2) {
      // wheel 진행여부 & wheel 방향 & page 끝지점 확인
			isWheeling = true;
			++nowPage;
			window.scrollTo({
				top: window.scrollY + window.innerHeight,
				left: 0,
				behavior: "smooth"	// scroll을 부드럽게 해줌. (safari 지원안함.)
			});
		} else if (!isWheeling && delta < 0 && nowPage > 0) {
			isWheeling = true;
			--nowPage;
			window.scrollTo({
				top: window.scrollY - window.innerHeight,
				left: 0,
				behavior: "smooth"
			});
		}
		timer = setTimeout(() => {
			isWheeling = false;
	  }, 100);
  	// wheel event가 끝나고 0.1초 뒤에 wheel 진행여부 변경.
	});
```

![Full Page Scroll](/Users/bandor/Documents/Projects/TIL/javascript/wheelEvent_fullpage-scroll.gif)

~~(너무 빠르게 wheel을 하다보면 동작하지 않을 수 있다..)~~

사파리 브라우저에선 scroll의 움직임이 보이지 않겠지만,
크롬에서 full page로 부드럽게 scroll 되는 것을 확인할 수 있다.



## Wheel Event를 활용한 animation

좀 더 심화학습으로 Wheel Event에 animation을 접목시켜보았다.

```html
<body>
  <div class="circle"></div>
  <h1 class="title">
    Hello, world!
  </h1>
</body>
```

```css
body {
  // 위의 코드와 동일.
}

.circle {
	width: 100px;
	height: 100px;
	background-color: black;
	border-radius: 50%;
}

.title {
	position: absolute;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -100%);
	color: white;
	opacity: 0;
	transition: opacity 0.5s linear;
	font-size: 3rem;
}

.appear {
	opacity: 1;
}

@keyframes zoomIn {
	30% {
		transform: scale(0.3);
	}
	100% {
		transform: scale(50);
	}
}

@keyframes zoomOut {
	0% {
		transform: scale(50);
	}
	70% {
		transform: scale(0.3);
	}
	100% {
		transform: scale(1);
	}
}
```

```javascript
const MIN_PAGE = 0;
const MAX_PAGE = 2;
let isWheeling = false;
let timer;
let nowPage = MIN_PAGE;

window.addEventListener("wheel", (e) => {
	e.preventDefault();
	clearTimeout(timer);
	const delta = e.wheelDelta;
  // 여기까진 동일.
  
	if (!isWheeling) {
		isWheeling = true;
		if (delta > 0) {
			++nowPage;
			switch (nowPage) {
				case 1:
					const circle = document.querySelector(".circle");
					circle.style.animation = "zoomIn  linear 1s forwards";
					break;
				case 2:
					const title = document.querySelector(".title");
					title.classList.add("appear");
					break;
				default:
					nowPage = MAX_PAGE;	// 최대 page를 넘기지 않도록 함.
					break;
			}
		} else {
			--nowPage;
			switch (nowPage) {
				case 0:
					const circle = document.querySelector(".circle");
					circle.style.animation = "zoomOut  linear 1s forwards";
					break;
				case 1:
					const title = document.querySelector(".title");
					title.classList.remove("appear");
					break;
				default:
					nowPage = MIN_PAGE;	// 최소 page를 넘기지 않도록 함.
					break;
			}
		}
	}
	timer = setTimeout(() => {
		isWheeling = false;
	}, 100);
});
```

![animation](/Users/bandor/Documents/Projects/TIL/javascript/wheelEvent_animation.gif)

스크롤마다 애니메이션이 단계적으로 진행되는 간단한 페이지를 만들어보았다.