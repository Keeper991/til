# gulp

자동화 도구로서, webpack에 비해 간편하다는 장점이 있다는 gulp.
~~(아직 webpack을 집중적으로 본 적이 없어서 비교군이 없다..)~~
(조만간 webpack과 gulp의 차이점을 정리해보도록 하겠다.)

task를 생성해 pipe에 연결해주면 gulp가 task를 물흐르듯 순차적으로 처리해준다.

task라 하면 트랜스파일링, 이미지 최적화, 파일변화감지, 빌드 등 여러가지가 있다.
gulp를 비롯한 자동화도구들은 이런 task들을 설정해놓고 run하면 알아서 처리해준다.
우리가 일일이 하나씩 할 필요없이.



## 설치

```console
npm i gulp
npm i gulp-cli -g
```

프로젝트 폴더에서 gulp를 설치한다. 그리고 전역으로 CLI 패키지도 설치해준다.

간혹 npm 버전이 낮으면 설치가 정상적으로 되지 않는 경우가 있다.

설치가 안된다면 체크해보자.



## 준비

gulp 설치를 완료했으면 터미널에서 `gulp`를 입력해보자.

gulpfile.js가 없다고 에러를 뱉는다면 성공이다.

에러의 이유를 알려줬으니 해결하면 된다.

```console
touch gulpfile.js
```



gulp역시 js문법을 따르고 있기 때문에, es6이상의 문법을 사용하고 싶다면
babel(js compiler)을 설치해 gulpfile과 연결해시켜줘야한다.

그리고 babel 사용을 위해 config 파일을 생성해주자.

```console
// gulpfile 이름 변경
mv gulpfile.js gulpfile.babel.js

// babel 사용을 위한 패키지 설치
npm i @babel/{core, register, preset-env}

// babel config 파일 생성
touch .babelrc
```



`.babelrc` 에 preset을 설정해주면 준비가 끝난다.
(preset은 babel 모듈들의 집합으로서, 다음 작업은 어떤 preset을 쓸 것인지 설정해주는 것이다.)

```json
{
  "presets": ["@babel/preset-env"] 
}
```

(`.babelrc`가 아니라 `babel.config.js`를 생성했다면, `module.export=` 을 추가해주면 된다.)
(`.babelrc`는 json, `babel.config.js`는 js로 형식이 다르기 때문에 이에 따라 작성해야 한다.)



## 사용

gulp 자체는 껍데기에 불과하다. 실질적인 작업은 open source로 제공되는 plugin들이 처리한다.

plugin은 직접 만들 수도 있고, 잘 만들어진 plugin을 가져다 쓸 수 있다.

예시로 pug파일을 html로 컴파일하는 gulpfile을 작성해보자.

```javascript
import gulp from "gulp";
import gpug from "pug";

const routes = {
  pug: {
    src: "src/*.pug",	// pug파일이 들어있는 디렉토리
    dest: "build"			// 컴파일된 html을 저장할 디렉토리
  }
}

const pug = () =>
	gulp
		.src(routes.pug.src)
		.pipe(gpug())
		.pipe(gulp.dest(routes.pug.dest));

export const dev = gulp.dest(pug);	// 반드시 함수가 아닌 변수로 export해줘야한다. 함수면 에러를 던진다..
```



위 코드대로라면 pug파일 변환하는 task 하나만 이뤄지고 있다.

여러 task를 처리하려면 맨 아래 코드 `gulp.dest()` 대신 `gulp.series()`를 사용하면 된다.



## 출처

* https://nomadcoders.co/gulp-for-beginners/

