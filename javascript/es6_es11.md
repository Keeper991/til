# ES6 ~ ES11 특이점

###### Update History

2020.12.23 - 최초 작성.



## spread syntax

* spread로 이뤄진 복사는 얕은 복사다.
* 같은 키를 가진 두 개의 object를 spread syntax로 하나의 object에 merge할 때,
  제일 뒤에 오는 키로 덮어씌워진다.

```javascript
const obj1 = { key: 'a' };
const obj2 = { key: 'b' };
const obj3 = { key: 'c' };
const objs = { ...obj1, ...obj2, ...obj3 }
console.log(objs); // { key: 'c' };
```



## Optional Chaining

object의 property에 접근할 때, 해당 property가 없을 경우에 발생하는 오류를 간편하게 처리할 수 있음.

```javascript
const person1 = {
  name: 'park',
  job: {
    title: 'baeksu',
    manager: {
      name: 'park'
    }
  }
};

const person2 = {
  name: 'park'
};

// 💩💩
{
  const printManager = (person) => {
    console.log(person.job.manager.name);
  }
  //printManager(person2);  // error
}

// 💩💩
{
  const printManager = (person) => {
    console.log(
      person.job
      ? person.job.manager
   	   ? person.job.manager.name 
       : undefined 
      : undefined);
  }
  printManager(person2);
}

// 💩💩
{
  const printManager = (person) => {
    console.log(person.job && person.job.manager && person.job.manager.name);
  }
  printManager(person2);
}

// 👍👍
{
  const printManager = (person) => {
    console.log(person.job?.manager?.name)
  }
  printManager(person2);
}
```



## Nullish Coalescing Operator

||(OR)을 이용해 다음과 같은 코드를 많이 작성한다.

```javascript
const name = '';
const userName = name || 'Guest';
console.log(userName);

const num = 0;
const message = num || 'undefined';
console.log(message);
```

`''`, `0`, `null`, `undefined` 는 `||` 의 입장에서 `false` 이므로,
위의 코드대로라면 userName과 message에 각각 Guest와 undefined가 할당되어야한다.

하지만 위 코드처럼 `||` 의 특성을 이용한 코드는 버그 발생 가능성이 크고,
사용자가 name에 빈값을, num이 0인 message를 원할 경우도 있기 때문에
아래와 같이 코드를 바꾸는 것이 좋다.

```javascript
const name = '';
const userName = name ?? 'Guest';
console.log(userName);

const num = 0;
const message = num ?? 'undefined';
console.log(message);
```