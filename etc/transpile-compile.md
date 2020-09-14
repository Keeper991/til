# Transpile vs Compile

두 개념을 혼용해서 많이 쓰인다고 한다.
(transpile을 transcompile이라 부르는 경우도 있다.)

나 역시 비슷한 늬앙스로 여겨, 상황에 따라 정확히 사용하기 위해 정리해본다.



둘의 공통점으로는 **변환**이라는 점이다.

기존의 데이터를 어떤 방향으로 바꾸는가에 따라 이 둘은 차이가 있다.

## Transpile

트랜스파일은 작성된 소스코드의 언어와 **비슷한 수준의 추상화를 가진 다른 언어로 변환**하는 것을 말한다.

예시로,

> es5 -> es6
>
> c++ -> c
>
> coffescript -> javascript

등이 있다.



## Compile

컴파일은 작성된 소스코드의 언어와 **다른 언어로 변환**하는 것을 말한다.

예시로,

> java -> bytecode
>
> c -> assembly

등이 있다.



## 정리

따라서, 컴파일이 더 큰 범주에 해당되고 트랜스파일은 그 안에 속한다고 볼 수 있다.



## 출처

* https://ideveloper2.tistory.com/166
* https://ooz.co.kr/416

