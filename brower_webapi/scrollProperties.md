# scroll 관련 properties

###### Update History

2021.03.07 - 최초 작성.





## scrollHeight

스크롤 하지 않은 element의 전체 높이.

![scrollHeight. 출처: MDN](https://developer.mozilla.org/@api/deki/files/840/=ScrollHeight.png)



## clientHeight

눈에 보이는 컨텐츠의 높이. border를 포함하지 않은 컨텐츠의 높이를 말함.

![clientHeight. 출처: MDN](https://developer.mozilla.org/@api/deki/files/608/=Clientheight.png)

## scrollTop

스크롤되어 올라간 만큼의 높이. element의 최상단부터 clientHeight로 계산되는 시작점까지의 높이.

![scrollTop. 출처: MDN](https://developer.mozilla.org/@api/deki/files/842/=ScrollTop.png)



## 끝까지 스크롤 했는 지 확인

> `clientHeight + scrollTop === scrollTop `



## 출처

* http://lab.naminsik.com/158
* https://developer.mozilla.org/ko/docs/Web/API/Element/scrollHeight