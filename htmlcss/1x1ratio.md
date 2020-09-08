# 1 x 1 Ratio



## 준비물

1. content box
2. wrapper



## 방법

`%` value가 container의 `width` 를 따른다는 점을 이용한다.

- html

```html
<div class="wrapper">
  <div class="content"></div>
</div>
```

- css

```css
.wrapper {
  width: 25%;
}

.content {
  padding-bottom: 100%; /* 1:1 ratio */
  /* padding-top을 사용해도 무방하다. */
}
```



## 다른 비율

* **2:1** : `padding-top: 50%`

* **1:2** : `padding-top: 200%`

* **4:3** : `padding-top: 75%`

* **16:9** : `padding-top: 56.25%`



## 참고

* https://webdir.tistory.com/487