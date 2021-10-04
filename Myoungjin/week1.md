# Ch1 - JavaScript 입문
## 1. 변수와 상수
### 변수
```js
let value = 1;
console.log(value); //1
value = 2;
console.log(value); //2

let value = 3; 
//SyntaxError : Identifier 'value' has already been declared
```
### 상수
```js
const a = 1; 
a = 2; //TypeError: "a" is read-only
```
### Data types
```js
let value = 1; //Number
let text = "hell0" //String
let isSame = true; //Boolean

const friend = null; //고의적으로 설정
let a;
console.log(a) //undefined
```

## 2. 연산자
### 산술연산자
- `+`, `-`, `*`, `/`
- `++`, `--`
### 대입연산자
- `=`
- `+=`, `-=`, `*=`, `/=`
### 논리연산자
- `!` : NOT
- `&&` : AND
- `||` : OR
- NOT, OR, AND 순으로 연산
### 비교연산자
- `===` : 양변의 값이 같은지 확인(type 또한 동일해야함)
- `==` : type과 관계 없이 값을 확인
    ```js
    let a = 1;
    let b = '1';
    console.log(typeof a); //number
    console.log(typeof b); //string
    console.log(a === b); //false
    console.log(a == b); //true
    ```
- `!==` : type check O
- `!=` : type check X

## 3. 조건문
- `if`, `if-else-elseif`, `switch-case`
- c와 상당히 유사

## 4. 함수
```js
function add(a, b) {
    return a + b;
}
console.log(add(1, 2));
```

### template literal (ES6)
```js
function hello(name) {
  console.log(`Hello, ${name}!`);
}
hello('velopert');
```

### 화살표 함수
```js
const add = (a, b) => {
    return a + b;
}
const multiply = (a, b) => a * b;
console.log(add(1, 2)); // 3
console.log(multiply(3, 2)); // 6
```

## 5. 객체
- 다른 객체지향언어에서의 객체와 동일한 개념
```js
const myDog = {
    name: '하늘이',
    age: 3,
    breed: 'black&tan pomeranian',
    sound: '멍멍',
    say: function() {
        console.log(this.sound);
    }
    /*
    say: () => console.log(this.sound)
    화살표 함수에는 this가 없기에 호출 된 곳에서의 this를 찾음 -> 오류
    */
};

```
- `getter`, `setter`
```js
let user = {
    name: 'Jhon',
    surname: 'Smith',

    get fullName() {
        return `${this.name} ${this.surname}`;
    },

    set fullName(value) {
        [this.name, this.surname] = value.split(" ");
    }
}

console.log(user.fullName); // call getter, Jhon Smith
user.fullName = "Alice Cooper"; // call setter
console.log(user.fullName); // call getter, Alice Cooper
```

## 6. 배열
```js
const exArr = [1, 3, 4.3, "a", {coffee: 'cold brew'}];
console.log(exArr); // [1, 3, 4.3, "a", Object]
console.log(exArr[4].coffee); // cold brew
console.log(exArr.length); // 5
exArr.push("Hi");
console.log(exArr.length); // 6
```

## 7. 반복문
```js
const arr = [];
for (let i = 0; i < 10; i++) {
    arr.push(i);
}
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 0~9
}
let i = 10;
while (i--) {
    console.log(i); // 9~0
}
for (let i of arr) {
    console.log(i); // 0~9
}

```

### Object
|method|return|
|-----|-----|
|`Object.entries`|`[[key1, value1], [key2, value2] ... ]`|
|`Object.keys`|`[key1, key2, key3 ... ]`|
|`Object.values`|`[value1, value2, value3 ... ]`|


```js
const myDog = {
    name: '하늘이',
    age: 3,
    breed: 'black&tan pomeranian',
    sound: '멍멍'
};

for (let key of Object.keys(myDog)) {
    console.log(key); // name  age  breed  sound
}

for (let key in myDog) {
  console.log(`${key}: ${myDog[key]}`);
}
```
### `break`, `continue`
C, Python과 동일

### 연습문제
```js
function biggerThanThree(numbers) {
  let result = [];
  for (let i of numbers) {
      if (i > 3) result.push(i);
  }
  return result;
}

const numbers = [1, 2, 3, 4, 5, 6, 7];
console.log(biggerThanThree(numbers)); // [4, 5, 6, 7]
```
## 8. 배열 내장함수(Method)
### forEach
```js
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];

superheroes.forEach(hero => {
  console.log(hero);
});
```

### map
```js
const array = [1, 2, 3, 4, 5, 6, 7, 8];

console.log(array.map(n => n * n));
```

### filter
```js
function biggerThanThree(numbers) {
  return numbers.filter(n => n>3);
}

const numbers = [1, 2, 3, 4, 5, 6, 7];
console.log(biggerThanThree(numbers)); // [4, 5, 6, 7]
```

### 찾기
|`method`|`return`|
|--|--|
|`arr.indexOf(a)`|arr에서 a의 인덱스|
|`objArr.findIndex(obj => obj.id===2)`|objArr에서 id가 2인 obj의 인덱스|
|`objArr.findIndex(obj => obj.id===2)`|objArr에서 id가 2인 obj|

### splice
특정 항목 제거
```js
const numbers = [10, 20, 30, 40];
const index = numbers.indexOf(30);
numbers.splice(index, 1);
console.log(numbers); // [10, 20, 40]
```

### slice
```js
const numbers = [10, 20, 30, 40];
const sliced = numbers.slice(0, 2); // [0,2)

console.log(sliced); // [10, 20]
console.log(numbers); // [10, 20, 30, 40]
```

### shift
첫 원소 추출

### pop
마지막 원소 추출

### unshift
맨 앞에 원소 추가

### concat
배열 합치기
```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const concated = arr1.concat(arr2);

console.log(concated); // [1, 2, 3, 4, 5, 6]
```

### join
```js
const array = [1, 2, 3, 4, 5];
console.log(array.join()); // 1,2,3,4,5
console.log(array.join(' ')); // 1 2 3 4 5
console.log(array.join(', ')); // 1, 2, 3, 4, 5
```

### reduce
```js
const numbers = [1, 2, 3, 4, 5];
let sum = array.reduce((accumulator, current) => accumulator + current, 0);

console.log(sum); // 15
```
```js
const numbers = [1, 2, 3, 4, 5];
let sum = numbers.reduce((accumulator, current, index, array) => {
  if (index === array.length - 1) {
    return (accumulator + current) / array.length;
  }
  return accumulator + current;
}, 0);

console.log(sum);
```

### 연습문제
```js
function countBiggerThanTen(numbers) {
  return numbers.reduce((acc, cur) => acc + cur>10, 0);
  // return numbers.filter(n => n>10).length
}

const count = countBiggerThanTen([1, 2, 3, 5, 10, 20, 30, 40, 50, 60]);
console.log(count); // 5
```

## 9. 프로토타입과 클래스
### 객체 생성자
```js
function Animal(type, name, sound) {
  this.type = type;
  this.name = name;
  this.sound = sound;
  this.say = function() {
    console.log(this.sound);
  };
}

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();
```
### 프로토타입
같은 객체 생성자 함수를 사용하는 경우, 특정 함수 또는 값을 재사용 할 수 있음<br/>
Array의 reduce, filter 등도 Array.prototype에 정의되어 있어 모든 배열에서 사용 가능한 것
```js
function Animal(type, name, sound) {
  this.type = type;
  this.name = name;
  this.sound = sound;
}

Animal.prototype.say = function() {
  console.log(this.sound);
};
Animal.prototype.sharedValue = 1;

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();

console.log(dog.sharedValue);
console.log(cat.sharedValue);
```
### 객체생성자의 상속
```js
function Dog(name, sound) {
  Animal.call(this, '개', name, sound);
}
Dog.prototype = Animal.prototype;
```

### class
```js
class Animal {
  constructor(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
  }
  say() {
    console.log(this.sound);
  }
}

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();
```
객체의 정의와 상속이 용이
### 상속 예시
```js
class Animal {
  constructor(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
  }
  say() {
    console.log(this.sound);
  }
}

class Dog extends Animal {
  constructor(name, sound) {
    super('개', name, sound);
  }
}

class Cat extends Animal {
  constructor(name, sound) {
    super('고양이', name, sound);
  }
}

const dog = new Dog('멍멍이', '멍멍');
const cat = new Cat('야옹이', '야옹');

dog.say();
cat.say();
```