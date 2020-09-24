# MVC

**M**odel
**V**iew
**C**ontroller



MVC는 Application을 3가지 역할로 구분한 개발 방법론이다.

사용자가 Controller를 조작하면 Controller는 Model을 통해서 데이터를 가져오고 그 정보를 바탕으로 시각적인 표현을 담당하는 View를 제어해서 사용자에게 전달하게 된다. 



사용자가 url을 입력하고 브라우저를 통해 해당 파일을 요청하면,
서버는 요청에 따른 처리(Controller) 과정을 거친다.
이 과정 안에는 요청하는 파일을 읽어오고(Model),
보여줄 형식을 갖춰서(View) 사용자의 요청에 응답하는 단계가 포함된다.



![MVC 흐름(출처: 생활코딩)](https://lh5.googleusercontent.com/-xAfIgOap9HI/UUcWtBm459I/AAAAAAAALvE/i6mJiOjIarg/s900/capture.gif "MVC 흐름(출처: 생활코딩)")



## 참고

* https://opentutorials.org/course/697/3828

  