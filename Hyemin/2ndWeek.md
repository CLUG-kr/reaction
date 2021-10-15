# 📢 2주차 스터디 일지

> 작성자 : 임혜민 날짜 : 2021.10.08 참여자 : 임혜민, 오명진, 김여진
> 

## ✅ JavaScript Intermediate

### ▶️ 2장 : 알고 있으면 유용한 JavaScript 문법

### 삼항연산자

---

❗Not ES6

if 조건문과 비슷하다. 

조건문 ? 참일때의 실행문 : 거짓일때의 실행문

형식으로 구성된다.

```jsx
condition ? When_true : When_false

let text = condition ? "it is true" : "it is false"
//you can assign
condition ? console.log("yes") : console.log("no")
//also acts like if statement
```

### Truthy/Falsy

---

에러 핸들링 할 때 알아두면 좋은 문법.

Falsy

- null
- undefined
- 0
- '' (빈 스트링)
- NaN (Not a Number)

위의 값들은 모두 false로 처리된다. = 앞에 !를 붙이면 true가 된다.

나머지 값들은 모두 truthy 로, true로 처리된다. = 앞에 !를 붙이면 false가 된다.

```jsx
function print(person) {
  if (!person) {
    console.log('person이 없네요');
    return;
  }
  console.log(person.name);
}

const person = null;
print(person);
```

### Short-circuit evaluation

---

논리 연산자를 이용하여 코드를 단축시키는 방법을 설명한다.

```jsx
const dog = {
  name: '멍멍이'
};

function getName(animal) {
  if (animal) {
    return animal.name;
  }
  return undefined;
}

const name = getName();
console.log(name);
```

위 코드는 함수 `getName` 에 객체가 주어지지 않은 경우, undefined를 리턴하도록 했다. 이것을 논리 연산자를 이용하여 아래와 같이 표현할 수 있다.

&& 연산자

 `&&` 연산자를 사용하는 경우, 앞 뒤 값의 특성에 따라 결과값이 달라진다.

`Truthy` && `Fasly` , `Falsy` && `Truthy` → Falsy

```jsx
console.log(true && '')
//출력값 : '' (falsy)
console.log(undefined && "hi")
//출력값 : undefined (falsy)
```

`Truthy` && `Truthy`  → 뒤에 있는 truthy 값

```jsx
console.log("hi" && "hello")
//출력값 : hello
console.log(6 && [1])
//출력값 : [1]
```

`Falsy` && `Fasly`  → 앞에 있는 Falsy 값

```jsx
console.log(0 && '')
//출력값 : 0
console.log(false && undefined)
//출력값 : false
```

따라서 아래의 코드는 객체가 제대로 들어와서 name 값을 조회할 수 있으면 `[animal.name](http://animal.name)` 을 리턴하고 그렇지 않으면 undefined를 리턴한다.

```jsx
const dog = {
  name: '멍멍이'
};

function getName(animal) {
  return animal && animal.name;
}

const name = getName();
console.log(name); // undefined
```

|| 연산자

`||` 연산자는 다음과 같이 기능한다.

`Truthy` && `Fasly` , `Falsy` && `Truthy` → Truthy

```jsx
console.log(undefined || "hi")
//출력값 : hi
console.log(true || '')
//출력값 : true
```

`Truthy` || `Truthy`  → 앞에 있는 truthy 값

```jsx
console.log(6 || [1])
//출력값 : 6
```

`Falsy` || `Fasly`  → 뒤에 있는 Falsy 값

```jsx
console.log(false || undefined)
//출력값 : undefined
```

따라서 아래의 코드는 객체가 제대로 들어오지 않은 경우 `name` 에 undefined가 할당되기 때문에 `Falsy` && `Truthy` 가 되어 "이름이 없는 동물입니다." 라는 문장이 출력되게 된다.

```jsx
const namelessDog = {
  name: ''
};

function getName(animal) {
  const name = animal && animal.name;
  return name || '이름이 없는 동물입니다.';
}

const name = getName(namelessDog);
console.log(name); // 이름이 없는 동물입니다.
```

### Default Parameter

---

❗ES6~

python에서 기본 인자 설정하는 것과 비슷하다.

```jsx
function calculateCircleArea(r = 1) {
  return Math.PI * r * r;
}

const area = calculateCircleArea();
console.log(area); // 3.141592653589793
```

위 코드에선 r을 입력하지 않아도 자동으로 1이 r에 대입된다.

```jsx
const calculateCircleArea = (r = 1) => Math.PI * r * r;

const area = calculateCircleArea();
console.log(area); // 3.141592653589793
```

기본 인자 설정은 화살표 함수에서도 가능하다.

### 조건문 심화

---

- 여러 값 비교

```jsx
function isAnimal(text) {
  return (
    text === '고양이' || text === '개' || text === '거북이' || text === '너구리'
  );
}
```

위와 같은 긴 비교문을 써야 할 때 배열과 배열의 `includes` 함수를 사용하면 코드가 간결해진다.

```jsx
function isAnimal(name) {
  const animals = ['고양이', '개', '거북이', '너구리'];
  return animals.includes(name);
}
```

배열 선언을 건너뛰고 화살표 함수로 작성도 가능하다.

```jsx
const isAnimal = name => ['고양이', '개', '거북이', '너구리'].includes(name);
```

- 값에 따라 다른 코드 실행하기

if문이나 switch 문을 사용할 수도 있지만, 객체를 활용하는 방법도 존재한다.

```jsx
function getSound(animal) {
  const sounds = {
    개: '멍멍!',
    고양이: '야옹~',
    참새: '짹짹',
    비둘기: '구구 구 구'
  };
  return sounds[animal] || '...?';
}
```

```jsx
function makeSound(animal) {
  const tasks = {
    개() {
      console.log('멍멍');
    },
    고양이() {
      console.log('고양이');
    },
    비둘기() {
      console.log('구구 구 구');
    }
  };
  if (!tasks[animal]) {
    console.log('...?');
    return;
  }
  tasks[animal]();
}
```

### 비구조화 할당

---

비구조화 할당이란?

> 객체 안에 있는 값을 추출해서 변수 혹은 상수로 바로 선언해줄 수 있는 문법. 함수 파라미터 안에서도 가능한다. (1장 섹션6)
> 

비구조화 할당에 대해서 좀 더 알아보자.

- 기본값 설정

앞에서 배운 함수 기본 인자 설정처럼, 비구조화 할당에서도 값이 들어오지 않을 때를 대비해 기본 값을 설정해줄 수 있다.

```jsx
//function parameter
const object = { a: 1 };

function print({ a, b = 2 }) {
  console.log(a);
  console.log(b);
}
//object
const object = { a: 1 };

const { a, b = 2 } = object;

console.log(a); // 1
console.log(b); // 2
```

- 이름 바꾸기

```jsx
const animal = {
  name: '멍멍이',
  type: '개'
};

const { name: nickname } = animal
console.log(nickname);
```

`const { name } = animal` 이 기본적인 비구조화 할당, 그런데 name이라는 이름을 쓰지 않고 nickname이라는 변수 이름을 쓰고 싶다면 `:` 를 이용하여 별칭을 만들어주면 된다.

- 배열 비구조화 할당

배열에서도 비구조화 할당이 가능하다. 배열 안에 있는 원소를 다른 이름으로 새로 선언해주고 싶을 때 사용하면 매우 유용하다.

```jsx
const array = [1, 2];
const [one, two] = array;
```

```jsx
const [one, two = 2] = array;
```

- 깊은 값 비구조화 할당

```jsx
const deepObject = {
  state: {
    information: {
      name: 'velopert',
      languages: ['korean', 'english', 'chinese']
    }
  },
  value: 5
};
```

위와 같은 객체가 있을 때, `name` , `language` , `value` 를 뽑아내고 싶다고 하자.

첫 번째 방법은 비구조화를 두 번 하는 것이다.

```jsx
const { name, languages } = deepObject.state.information;
const { value } = deepObject;
```

두 번째 방법은 한 번에 모두 추출하는 것이다.

```jsx
const {
  state: {
    information: { name, languages }
  },
  value
} = deepObject;
```

### Object - shorthand

---

❗ES6~

객체 생성 시 key값과 동일한 이름으로 생성된 변수가 있으면 바로 매칭시켜주는 문법이다.

위에서 `name` , `language` , `value` 를 뽑아서 바로 동일 이름의 key값을 가진 객체에 넣고 싶을 때 아래와 같이 사용할 수 있다.

```jsx
const extracted = {
  name,
  languages,
  value
}
```

### Spread and Rest

---

❗ES6~

spread 연산자

- 객체

```jsx
const slime = {
  name: '슬라임'
};

const cuteSlime = {
  ...slime,
  attribute: 'cute'
};

const purpleCuteSlime = {
  ...cuteSlime,
  color: 'purple'
};
```

- 배열

```jsx
const animals = ['개', '고양이', '참새'];
const anotherAnimals = [...animals, '비둘기'];
console.log(animals);
console.log(anotherAnimals);
```

참조 아님. 기존 것은 건드리지 않기 때문에 복사에 가까움.

- 함수 인자

parameter 는 매개변수, argument가 인자다.

```jsx
function add (a, b){
	return a + b;
}
//a, b is parameter
add(1, 3);
//1, 3 is argument
```

배열의 원소들을 모두 인자로 넣어주고 싶을 때 spread를 쓸 수 있다.

```jsx
const numbers = [1, 2, 3, 4, 5, 6];
const result = sum(...numbers);
```

`[1, 2, 3, 4, 5, 6]` 이 각각 `1, 2, 3, 4, 5, 6` 으로 `sum` 에 전달된다.

rest 연산자

- 객체

```jsx
const purpleCuteSlime = {
  name: '슬라임',
  attribute: 'cute',
  color: 'purple'
};

const { color, ...rest } = purpleCuteSlime;
// rest : {name: "슬라임", attribute: "cute"}
```

- 배열

```jsx
const numbers = [0, 1, 2, 3, 4, 5, 6];

const [0, ...rest] = numbers;
//const [...rest, one] = numbers; 는 안됨
```

객체와 배열에서 쓰이는 rest 문법은 비구조화 할당과 같이 쓰인다.

- 함수 파라미터

함수의 파라미터가 몇 개가 될 지 모르는 상황에서 rest를 쓰면 매우 유용하다.

```jsx
function sum(...rest) {
  return rest.reduce((acc, current) => acc + current, 0);
}

const result = sum(1, 2, 3, 4, 5, 6);
//result : 21
```

`sum` 에 쓰인 `...rest` 는 1, 2, 3, 4, 5, 6 을 [1, 2, 3, 4, 5, 6]으로 전달한다.

n개의 숫자들이 파라미터로 주어졌을 때, 그 중 가장 큰 값을 알아내기

→ rest 사용.

```jsx
function max(...nums) {
  let maxval = nums.reduce((accum, curr) => {
    if(accum < curr){
      accum = curr;
    }
    return accum;
  }, nums[0])
  return maxval;
}
```

Spread

> 객체 혹은 배열의 원소를 하나 씩 펼쳐내줌
> 

[1, 2] → 1, 2

Rest

> 여러 개의 원소들을 하나의 묶음으로 묶어줌
> 

1, 2 → [1, 2]

### Scope

---

scope의 종류

- global scope
- function scope
- block scope

`let` , `const` 와 `var` 의 차이

let과 const는 block scope로, var는 function scope로 선언된다.

→ if, for, switch 등에서 선언한 값이 그 밖에서도 영향을 미친다.

```jsx
var value = 'hello!';

function myFunction() {
  var value = 'bye!';
  if (true) {
    var value = 'world';
    console.log('block scope: ');
    console.log(value); //value: world
  }
  console.log('function scope: ');
  console.log(value); //value: world
}

myFunction();
console.log('global scope: ');
console.log(value); //value: hello
```

이외에도 let, const, var의 차이점은 다음과 같다.

> 1. var는 이미 선언되어있는 이름과 같은 이름으로 변수를 또 선언해도 에러가 나지 않지만, let과 const는 같은 이름의 변수를 또 선언할 수 없다.

2. var, let은 변수 선언 시 초기값을 주지 않아도 되지만 const는 반드시 할당해야 한다.

3. var, let은 값을 다시 할당할 수 있지만 const는 상수이므로 한 번 할당한 값을 변경할 수 없다.

4. var로 선언한 변수는 선언 전에 사용해도 에러가 나지 않지만, let과 const는 에러가 발생한다.
> 

이 중 4번을 Hoisting이라고 한다.

Hoisting

> 아직 선언되지 않은 함수 및 변수를 끌어올려서 사용할 수 있는 JS만의 작동 방식
> 

```jsx
myFunction();

function myFunction() {
  console.log('hello world!');
}
```

변수도 Hoisting될 수 있다.

```jsx
console.log(number);
var number = 2;
```

앞에서 var는 선언 전 사용해도 에러가 나지 않지만, let과 const는 다르다고 했다. 그것은 var는 Hoisting이 되며 자동으로 undefined가 초기값으로 할당되고, let과 const는 Hoisting 시 자동 초기값 할당이 되지 않기 때문이다.

변수가 선언되고 해당 변수에 값이 할당되기 전까지를 TDZ(Temporal Dead Zone)이라고 한다. 

```jsx
console.log(num); // Uncaught ReferenceError: num is not defined
let num = 1;
console.log(num); //1
```

위의 코드를 Hoisting version으로 다시 보면,

```jsx
let num;
console.log(num); //TDZ.
num = 1;
console.log(num);
```

두 번째 줄에서 num은 아직 값이 할당되지 않았으므로 메모리에 존재하지 않는 상태이다.

### ▶️ 3장 : JavaScript에서 비동기 처리 다루기

어떤 작업이 실행되고 있을 때, 동시에 다른 작업도 할 수 있도록 하는 작업 방식.

```jsx
function work() {
  const start = Date.now();
  for (let i = 0; i < 1000000000; i++) {}
  const end = Date.now();
  console.log(end - start + 'ms');
}

work();
console.log('다음 작업');
```

위 코드의 출력값은 다음과 같다.

(work 함수 실행 시간)ms

다음 작업

시간이 오래 걸리는 `work()` 함수. 이 함수가 실행되는 동안 다른 작업도 하고 싶다면?

```jsx
function work() {
  setTimeout(() => {
    const start = Date.now();
    for (let i = 0; i < 1000000000; i++) {}
    const end = Date.now();
    console.log(end - start + 'ms');
  }, 0);
}

console.log('작업 시작!');
work();
console.log('다음 작업');
```

`setTimeout` 이라는 함수를 사용해야 한다. 첫 번째 파라미터는 실행되기 원하는 작업을 담은 함수, 두 번째 파라미터는 작업이 실행될 시간을 넣는다(ms 단위). 0을 넣으면 0ms 이후에 실행된다는 뜻이다. 이 함수를 이용하면 `work()` 함수가 백그라운드에서 실행되기 때문에 다른 작업도 동시에 실행할 수 있다.

위 코드의 출력값은 다음과 같다.

작업 시작!

다음 작업

(work 실행 시간)ms

```jsx
function work(callback) {
  setTimeout(() => {
    const start = Date.now();
    for (let i = 0; i < 1000000000; i++) {}
    const end = Date.now();
    console.log(end - start + 'ms');
    callback();
  }, 0);
}

console.log('작업 시작!');
work(() => {
  console.log('작업이 끝났어요!')
});
console.log('다음 작업');
```

work() 함수가 끝나자마자 어떤 작업이 실행되도록 하고 싶다면, 콜백함수를 파라미터로 넣어주면 된다. 그러면 콜백함수를 전달받은 함수가 실행이 끝나면 해당 콜백함수가 실행된다.

따라서 출력값은 다음과 같아진다.

작업 시작!

다음 작업

(work함수 실행시간)ms

작업이 끝났어요!

### Promise

---

❗ES6~

비동기 처리를 콜백함수로 하지 않고도 편하게 처리할 수 있도록 도입된 기능

```jsx
function increaseAndPrint(n, callback) {
  setTimeout(() => {
    const increased = n + 1;
    console.log(increased);
    if (callback) {
      callback(increased);
    }
  }, 1000);
}

increaseAndPrint(0, n => {
  increaseAndPrint(n, n => {
    increaseAndPrint(n, n => {
      increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
          console.log('끝!');
        });
      });
    });
  });
});
```

위와 같이 콜백함수를 반복적으로 불러야하는 경우, 코드가 난잡해지는 경우를 방지하기 위해 promise를 도입하였다.

```jsx
const myPromise = new Promise((resolve, reject) => {
  // 구현..
})
```

promise의 기본 형태는 위와 같다. promise가 성공하면 resolve를 호출하고, 실패하면 reject를 호출한다. resolve는 성공 시 결과값을, reject는 실패 시 오류를 설정한다.

```jsx
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(1);
  }, 1000);
});

myPromise.then(n => {
  console.log(n);
});
```

promise를 통해 1초(1000ms) 뒤 성공으로 처리하고 1을 resolve를 통해 작업이 끝난 후 다시 쓸 수 있게 한 후 출력하는 코드이다. 작업이 끝나고 나서 또 다른 작업을 해야 할 경우 then을 사용한다.

```jsx
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(new Error());
  }, 1000);
});

myPromise
  .then(n => {
    console.log(n);
  })
  .catch(error => {
    console.log(error);
  });
```

promise를 통해 1초 뒤 실패로 처리하고 Error 객체를 reject를 통해 다시 쓸 수 있게 한 후 출력하는 코드이다. 작업이 실패했을 때 수행할 작업에 대해서는 catch를 사용한다.

```jsx
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0).then((n) => {
  console.log('result: ', n); //출력값: 1
})
```

resolve와 reject경우를 모두 포함한 promise 코드이다. 전달받은 수를 +1 해서 돌려주는 기능을 하고, 만약 `increaseAndPrint(4)`를 하면 `ValueIsFiveError` 가 발생한다.

promise가 콜백 함수를 통한 비동기적 처리와 달라지는 지점은 지금부터이다. promise를 연달아서 사용할 때다.

```jsx
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .catch(e => {
    console.error(e);
  });
```

이렇게 하면 1초에 한 번씩 전달받은 수에 1을 더할 수 있다. 마지막에 value가 5가 되므로, then 대신 catch가 걸려서 에러가 나는 것 까지 볼 수 있다.

그리고 좀 더 깔끔하게 정리하면

```jsx
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;
      if (value === 5) {
        const error = new Error();
        error.name = 'ValueIsFiveError';
        reject(error);
        return;
      }
      console.log(value);
      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .catch(e => {
    console.error(e);
  });
```

then에 넣는 것은 함수이기 때문에, 화살표 함수로 선언하는 대신 `increaseAndPrint`를 넣으면 resolve를 통해 전달받은 값을 그대로 다시 `increaseAndPrint`에 넣어서 다시 실행하게 된다.

### async and await

---

❗ES8~

promise를 더 쉽게 사용할 수 있게 해준다.

```jsx
fetch('coffee.jpg')
.then(response => response.blob())
.then(myBlob => {
  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
})
.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});
```

위의 코드는 promise를 이용하여 fetch → response.blob() → then/catch 작업을 실행하는 코드이다. 이것을 async와 await를 통해 다시 써보면

```jsx
async function myFetch() {
  let response = await fetch('coffee.jpg');
  let myBlob = await response.blob();

  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}

myFetch()
.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});
```

위 코드와 같이 된다.

async function()은 await가 비동기 코드를 호출할 수 있게 해주는 함수다. async 함수는 결과값으로 promise를 반환한다. 

await는 promise 기반 함수 앞에 놓을 수 있다. 그리고 promise가 성공 혹은 실패할 때까지 중단하고, 결과를 반환한다. 위에 promise만 쓴 코드와 비교하여 줄줄이 이어지는 then이 없어졌으므로 좀 더 깔끔해졌다.

```jsx
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function makeError() {
  await sleep(1000);
  const error = new Error();
  throw error;
}

async function process() {
  try {
    await makeError();
  } catch (e) {
    console.error(e);
  }
}

process();
```

async에서 에러를 발생시킬 때는 throw를, 잡아낼 때에는 try-catch를 이용한다.

async를 이용한 비동기적 함수들을 동시에 실행시키고 싶을 때는, promise.all을 사용한다.

```jsx
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const getDog = async () => {
  await sleep(1000);
  return '멍멍이';
};

const getRabbit = async () => {
  await sleep(500);
  return '토끼';
};
const getTurtle = async () => {
  await sleep(3000);
  return '거북이';
};

async function process() {
  const results = await Promise.all([getDog(), getRabbit(), getTurtle()]);
	const [dog, rabbit, turtle] = await Promise.all([getDog(), getRabbit(), getTurtle()]);
  console.log(results, dog, rabbit, turtle);
}

process();
```

promise.all의 리턴값은 ['멍멍이', '토끼', '거북이']의 배열 형식이다. promise.all로 받아온 결과값들을 비구조화 할당으로 하나씩 받을 수도 있다. promise.all에서 등록한 프로미스 중 하나라도 실패하면 모든 것이 실패한 것으로 간주된다.

```jsx
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const getDog = async () => {
  await sleep(1000);
  return '멍멍이';
};

const getRabbit = async () => {
  await sleep(500);
  return '토끼';
};
const getTurtle = async () => {
  await sleep(3000);
  return '거북이';
};

async function process() {
  const first = await Promise.race([
    getDog(),
    getRabbit(),
    getTurtle()
  ]);
  console.log(first); //first: "토끼"
}

process();
```

promise.race는 all과 달리 제일 빨리 끝난 프로미스의 값을 가져온다. 그리고 제일 빨리 끝난 getRabbit()에서 에러가 발생하면 잡아낼 수 있지만, 늦은 프로미스들에서 발생한 에러는 무시된다.

### ▶️ 4장 : HTML과 JavaScript 연동하기

### 카운터

---

버튼을 클릭하면 숫자가 올라가거나 내려가는 카운터를 html에 구현해 보자.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Parcel Sandbox</title>
    <meta charset="UTF-8" />
  </head>

  <body>
    <h2 id="number">0</h2>
    <div>
      <button id="increase">+1</button>
      <button id="decrease">-1</button>
    </div>

    <script src="src/index.js"></script>
  </body>
</html>
```

index.html

```jsx
const number = document.getElementById("number");
const increase = document.getElementById("increase");
const decrease = document.getElementById("decrease");

increase.onclick = () => {
  const current = parseInt(number.innerText, 10);
  number.innerText = current + 1;
};

decrease.onclick = () => {
  const current = parseInt(number.innerText, 10);
  number.innerText = current - 1;
};
```

index.js

### 모달

---

버튼을 클릭하면 기존의 내용을 가리면서 나오는 팝업 메시지를 구현해 보자.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Parcel Sandbox</title>
    <meta charset="UTF-8" />
  </head>

  <body>
    <h1>안녕하세요!</h1>
    <p>내용내용내용</p>
    <button id="open">버튼 열기</button>
    <div class="modal-wrapper" style="display: none;">
      <div class="modal">
        <div class="modal-title">안녕하세요</div>
        <p>모달 내용은 어쩌고 저쩌고..</p>
        <div class="close-wrapper">
          <button id="close">닫기</button>
        </div>
      </div>
    </div>
    <script src="src/index.js"></script>
  </body>
</html>
```

index.html

```css
body {
  font-family: sans-serif;
}

.modal-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  background: white;
  padding: 24px 16px;
  border-radius: 4px;
  width: 320px;
}

.modal-title {
  font-size: 24px;
  font-weight: bold;
}

.modal p {
  font-size: 16px;
}

.close-wrapper {
  text-align: right;
}
```

styles.css

```css
import "./styles.css";

const open = document.getElementById("open");
const close = document.getElementById("close");
const modal = document.querySelector(".modal-wrapper");
open.onclick = () => {
  modal.style.display = "flex";
};
close.onclick = () => {
  modal.style.display = "none";
};
```

index.html
