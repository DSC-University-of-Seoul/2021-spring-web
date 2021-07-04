# Javascript 다시 공부

## Javascript setting

### API(Application Programming Interface)

node.js, 웹 API가 모두 console에 관련된 API가 있으며 이 둘의 인터페이스는 동일
console은 언어자체에 포함되지 않지만 통상적으로 많이 쓰이기 때문이다.
웹 API는 자바스크립트 언어 자체에 포함된 것이 아니라
브라우저가 제공하고 이해할 수 있는 함수들을 말한다.  

### develope tool

#### console

콘솔 탭 안에서 코드에서 작성한 메세지 확인 및 자바스크립트 실행 가능
-> 동적으로 요소들을 검사하고 붙여놓고 지울 수 있음  

#### source

break point를 걸어서 디버깅할 때 유용하게 쓰임  

#### network

네트워크가 얼마나 발생하고
얼마나 많은 데이터가 발생하는지 확인 가능  

### 유용한 사이트

[자바스크립트 공식사이트](notion://www.notion.so/ecma-international.org)

[자바스크립트 모질라](notion://www.notion.so/developer.mozilla.org)

### HTML 읽기

#### head

parsing HTML -> (blocked - feching js -> executing js) -> parsing HTML  
단점: 이렇게 됐을 때 js 파일이 크면 사용자가 기다리는 시간 늘어남  

### body tail

parsing HTML -> fetching js executing js  
단점: 사용자가 기본적인 HTML을 빨리 보지만 자바스크립트에 의존적인
웹사이트라면 사용자가 정상적인 페이지를 보기 전 까지는 기다려야 함  

### head + async

`<script async src="a.js"></script>`  
async는 boolean type으로 선언하는 것만으로도 True로 선언되서 사용  

parsing +fetchingjs -> (blocked - execting js) -> parsing HTML  
장점: 다운로드 받는 시간 절약  
단점: HTML이 parsing이 되기 전에 자바스크립트가 실행되기 때문에  
실행과정 상 문제가 생길 수 도 있으며, 여전히 blocked 된 구간이 있어 시간이 오래걸림  

### head + defer

parsing HTML + fetching js -> executing js  
`<script defer src="a.js"></script>`  
가장 이상적인 환경  

### 시작 전 선언

바닐라 자바스크립트를 작성할 때
`'use strict';`를 선언하는 것이 좋음
자바스크립트는 유연해서 개발자가 많은 실수를 할 수 있음
이러한 비상식적인 것을 쓸 수 없도록 제한  

## Javascript

### 데이터 타입

```jsx
// 1. Use strict
// added in ES 5
// Use this for Vanilla Javascript

'use strict';

// 2. Variable, rw(read/write)
// let (added in ES6)

// global 변수(블럭 밖)는 어플리케이션이 실행되고 끝날때까지 실행되서 최소한으로 쓰는 것이 좋음
// 가능한 class나 함수, if, for로 필요한 부분에서만 정의해서 쓰기

// 자바스크립트에서 변수를 실행시킬 수 있는 딱 하나의 방법 'let' EX6에 추가됨
// var을 변수 선언하기 위해 쓴다면 값을 선언하기도 전에 쓸 수도 있어서 문제가 생기며 블럭 스콥도 가지지 않음
// var hoisting (move declaration from bottom to top)
// var has no block scope

let globalName = 'global name'; 

{
    let name = 'ellie';
    console.log(name);
    name = 'hello';
    console.log(name);
    console.log(globalName);
    
}
console.log(name);
console.log(globalName);

// 3. Constant, r(read only)
// 값이 한번 할당 시 바뀌지 않음 (immutable) <-> mutable 'let'
// Note!
// Immutable data types: primitive types, frozen objects (i.e. object.freeze())
// Mutable data types: all objects by default are mutable in JS
// favor immutable data type always for a few reason:
//  - security
//  - thread safety (다양한 thread가 접근해서 값이 변하지 않는 것이 좋음)
//  - reduce human mistakes

const daysInWeek = 7;
const maxNumber = 5;

// 4. Variable types
// primitive, signle item: number, string, boolean, null, undefined, symbol
// 값 자체가 메모리에 저장
// Object, box container
// 너무 커서 메모리에 한번에 다 올라갈 수 없고, 오브젝트가 담겨있는 reference가 저장되고 
// reference 와 실제 오브젝트 담겨 있는 메모리가 연결되어 있음
// function, first-class function (함수도 데이터 타입 중 하나 - 함수의 인자로도 전달, 함수의 return type으로 함수를 전달할 수 있음 )
// 자바스크립트의 경우 숫자는 number만쓰고 심지어 선언하지도 않아도 됨

const count = 17; // integer
const size = 17.1; // decimal numger
console.log(`value: ${count}, type: ${typeof count}`);
console.log(`value: ${size}, type: ${typeof size}`);

// number - special numeric values: infinity, -infinity, Not a Number
const infinity = 1 / 0;
const negativeInfinity  = -1 / 0;
const nAn = 'not a number' / 2; // NA값 출력
console.log(infinity);
console.log(negativeInfinity);
console.log(nAn);

// bigInt (fairly new, don't use it yet) - 기존 범위를 벗어나는 숫자 데이터 타입 (크롬, 파이어폭스만 지원)
const bigInt = 1231314332532224242404023593573257n; // over (-2**53) ~ (2*53)
console.log(`value: ${bigInt}, type: ${typeof bigInt}`);

// stirng
const char = 'c';
const brendan = 'brendan';
const greeting = 'hello' + brendan;
console.log(`value: ${greeting}, type: ${typeof greeting}`);
const helloBob = `hi ${brendan}!;` //template literals (Stirng) `원하는 string ${변수}` -> ${변수}에는 변수 값이 나옴
console.log(`value: ${helloBob}, type: ${typeof helloBob}`);

// bollean
// false: 0, null, undefined, NaN, ''
// true: any other value
const canRead = true;
const test = 3 < 1; //false
console.log(`value: ${canRead}, type: ${typeof canRead}`);
console.log(`value: ${test}, type: ${typeof test}`);

// null
let nothing = null;
console.log(`value: ${nothing}, type: ${typeof nothing}`);

// undefined 아무것도 지정되지 않은 상태 혹은 undefined로 지정
let x = undefined;
console.log(`value: ${x}, type: ${typeof x}`);

// symbol, create uniwue identifiers for objects 
// 맵이나 다른 자료구조에서 고유한 식별자가 필요하거나 
// 아니면 동시다발적으로 일어날 수 있는 코드에 우선적으로 주고 싶을 때 고유 id 부여
// 주어진 식별자와 상관없이 다른 심볼로 만들어짐
const symbol1 = Symbol('id');
const symbol2 = Symbol('id');
console.log(symbol1 === symbol2);
// 식별자가 똑같다면 동일한 심볼을 만들고 싶을 때
const gsymbol1 = Symbol.for('id');
const gsymbol2 = Symbol.for('id');
console.log(gsymbol1 === gsymbol2);
// 바로 출력하면 에러나서 .description으로 string으로 변환시켜야 함
console.log(`value: ${symbol1.description}, type: ${typeof Symbol1}`);

// object, real-life object, data structure
const ellie = {name: 'ellie', age: 20}; // ellie는 변환 불가하지만 ellie object 안의 변수는 변환 가능
ellie.age = 21;

// 6. Dynamic typing: dynamically typed language
let text = 'hello';
console.log(`value: ${text}, type: ${typeof text}`);
text = 1;
console.log(`value: ${text}, type: ${typeof text}`); // 숫자열로 변환
text = '7' + 5;
console.log(`value: ${text}, type: ${typeof text}`); // 스트링으로 변환
text = '8' / '2';
console.log(`value: ${text}, type: ${typeof text}`); // 숫자와 숫자가 들어있는 스트링이라 숫자열로 변환

// 런타임에 타입이 정해져서 문제가 발생한 경우가 많음
// 이러한 이유로 TS가 나오게 됨!!
```

### Operator, if, loop

```jsx
// 1. String concatenation
console.log('my' + 'cat');
console.log('1' + 2);
console.log(`string literals:

''''
1+2 = ${1 + 2}`);

// 특수문자 그대로 - \특수문자 
// enter - \n
// tab = \n\

console.log('ellie\'s \nbook');

// 2. Numeric operators
console.log(1 + 1); //add
console.log(1 - 1); //substract
console.log(1 / 1); //divide
console.log(1 * 1); //multiply
console.log(1 % 1); //remainder
console.log(1 ** 1); //exponentation

// 3. Increment and decrement operators
let counter = 2;
const preIncrement = ++counter;
// counter = counter + 1;
// preIncrement = counter;
console.log(`preIncrement: ${preIncrement}, counter: ${counter}`);
const postIncrement = counter++;
// postIncrement = counter;
// counter = counter + 1;
console.log(`preIncrement: ${postIncrement}, counter: ${counter}`);
const preDecrement = --counter;
// counter = counter - 1;
// preDecrement = counter;
console.log(`preIncrement: ${postIncrement}, counter: ${counter}`);
const postDecrement = counter--;
// postDecrement = counter;
// counter = counter - 1;
console.log(`preDecrement: ${postDecrement}, counter: ${counter}`);

// Assignment operators
let x = 3;
let y = 6;
x += y; // x = x + y;
x -= y;
x *= y;
x /= y;

// 5. Comparison operators
console.log(10 < 6); // less than
console.log(10 <= 6); // less than or equal
console.log(10 > 6); // greater than
console.log(10 >= 6); // greater than or equal

// 6. Logical operators: || (or), && (and,), ! (not)
const value1 = false;
const value2 = 4 < 2;

// || (or), finds the first truthy value 
// or이 true면 멈추기 때문에 계산이 복잡한 것을 제일 뒤에 두어야 함 
console.log(`or: ${value1 || value2 || check()}`);

function check(){
    for (let i = 0; i < 10; i++) {
        // wasting time
        console.log('😨');
    }
    return true;
}

// && (and), finds the first falsy value
// and 역시 하나라도 false면 멈추기 때문에 제일 연산이 복잡한 것을 뒤에 두어야 함
console.log(`and: ${value1 && value2 && check()}`);

function check(){
    for (let i = 0; i < 10; i++) {
        // wasting time
        console.log('😨');
    }
    return true;
}

// null 체크할 때 and로 확인
// oftne used to compress long if-statement
// nullableObject && nullableObject.something
// if (nullableObject != null) {
//     nullableObject.something;
// }

// ! (not)
console.log(!value1);

// 7. Equality
const stringFive = '5';
const numberFive = 5;

// == loose equality, with type conversion
console.log(stringFive == numberFive);
console.log(stringFive != numberFive);

// === strict equality, no type conversion
console.log(stringFive === numberFive);
console.log(stringFive !== numberFive); 

// object equality by reference
const ellie1 = {name: 'ellie'};
const ellie2 = {name: 'ellie'};
const ellie3 = ellie1;
console.log(ellie1 == ellie2); // 다른 reference라서 false
console.log(ellie1 === ellie2); // reference 값이 달라 false
console.log(ellie1 === ellie3);

// equality - puzzler
console.log(0 == false); // true
console.log(0 === false); // false
console.log('' == false); // true
console.log('' === false); // false
console.log(null == undefined); // true
console.log(null === undefined); // false

// 8. Conditional operators: if
// if, else if, else
const name = 'df'; 
if (name === 'ellie') {
    console.log('Welcome, Ellie!');
} else if (name === 'coder') {
    console.log('You are amazing coder');
} else {
    console.log('unknown');
}

// 9. Ternary operator: ? (간단할 때)
// condition ? value1: value2; 
console.log(name === 'ellie' ? 'yes' : 'no');

// 10. Switch statement
// use for multiple if checks
// use for enum-like value check
// use for multiple type checks in TS
const browser = 'IE';
switch (browser) {
    case 'IE':
        console.log('go away!');
        break;
    case 'Chrome':
    case 'Firefox':
        console.log('love you!');
        break;
    default:
        console.log('same all!');
        break;
}

// 11. lOOPS
// while loop, while the condition is truthy,
// body code is executed.
let i = 3;
while (i > 0) {
    console.log(`while: ${i}`);
    i--;
}

// do while loop, body code is executed first,
// then check the conditions.
do {
    console.log(`do while: ${i}`);
    i--;
} while (i > 0);

// for llop, for(begin; condition; step)
for (i = 3; i > 0; i--) { // 기존에 있는 변수를 사용
    console.log(`for: ${i}`) ;
}

for (let i = 3; i > 0; i = i -2) {
    // inline variable declaration
    console.log(`inline variable for: ${i}`);
}

// nested loops 그러나 빅오가 n^2라서 별로 좋지는 않음
for (let i = 0; i < 10; i++) {
    for (let j = 0; j < 10; j++) {
        console.log(`i: ${i}, j:${j}`);
    }
}

// break(루프 완전히 끝내기), continue(루프 지금것만 스킵하고 다음 루프로)
// Q1. iterate from 0 to 10 and print only even numbers
// (use continue)

for (let k = 0; k < 11; k++) {
    if (k % 2 !== 0) {
        continue;
    } 
    console.log(`q1. ${k}`);
}

// Q2. iterate from 0 to 10 and print numbers until reaching 8
// (use break)

for (let k = 0; k < 11; k++) {
    if (k === 8) {
        break;
    }
    console.log(`q2. ${k}`);
}    

// 루프 공급할 때 레이블도 있음 근데 bad smell이 난다!
```

### function

```jsx
// prgram 내부에 functon 존재 
// Javascript는 절차지향 언어인데 절차지향 언어는 함수가 매우 중요
// input -> output, 언어자체 function, API를 사용할 때 이 함수의 이름을 보고 함수의 기능과 파라미터, 리턴 값을 예상할 수 있다.

// Function
// - fundamental building block in the program 
// - subprogram can be used multiple times (재사용 가능)
// - performs a task or calculates a value 

// 1. function declaration
// function name(param1, param2) { body... return;}
// one function === one thing (하나의 함수는 한 가지 일만 하도록)
// naming: doSomething, command, verb (동작하는 기능) 
// e.g. createCardAndPoint -> creatCard, creatPoint (두 가지가 있다면 세분화하는 것을 고민)
// function is object in JS (자바스크립트에서 function은 object - 변수에 할당 가능, 파라미터 전달, 함수 리턴 가능) -> 함수가 object라서 속성값 적용 가능
function printHello() {
    console.log('hello');
}
printHello();
function log(message) {
    console.log(message);
}
log('Hello');
// JS에는 타입이 없어서 메세지가 스트링인지 숫자인지 명확하지 않음 -> 혼란 (TypeScript는 명시 -> 웹페이지에서 사용해보기)

// 2. parameters
// premitive parameters: passed by value
// object parameters: passed by reference
function changeName(obj) {
    obj.name = 'coder';
}
const ellie = {name: 'ellie'};
changeName(ellie);
console.log(ellie);
// 오브젝트는 레퍼런스로 전달되어 함수 안에서 오브젝트의 값을 변경하게 되면 변경된 사항 그대로 메모리 적용 추후 확인 가능

// 3. default parameters (added in ES6)
function showMessage(message, from = 'unknown') {
    console.log(`${message} by ${from}`);
}
showMessage('Hi!');

// 4. rest parameters (added in ES6)
function printAll(...args) { // array 형태
    for (let i = 0; i < args.length; i++) {
    console.log(arg[i]);
    }

    for (const arg of args) { // simple하게 적용
        console.log(arg);
    }

    args.forEach((arg) => console.log(arg)); // more simple하게 적용
}
printAll('dream', 'coding', 'ellie');

// 5. local scope
let globalMessage = 'global'; // global variable
function printMessage() {
    let message = 'hello';
    console.log(message); // local variable (밖에서 출력하면 error)
    console.log(globalMessage);
    function printAnother() {
        console.log(message);
        let childMessage = 'hello'; // 자식 안의 변수도 자식 안에서만
    }
}
printMessage();
// 밖에서는 안이 보이지 않고, 안에서만 밖을 볼 수 있다는 원칙 지키기

// 6. return a value
function sum(a, b){
    return a + b;
}
const result = sum(1,2); // 3
console.log(`sum: ${sum(1, 2)}`);
// return이 없으면 return = undefined; 자동적으로 들어가 있음

// 7. early return, early exit (조건이 맞지 않을 때 return 해서 빨리 함수 종료시킥도록)
// bad
function ungradeUser(user) {
    if (user.point > 10) {
        // long upgrade logic ...
    }
}
// good
function upgradeUser(user) {
    if (user.point <= 10) {
        return;
    }
    // long upgrade logic...
}

// Furst-class function
// functions are treated like any other variable 함수는 변수에 할당이 되며
// can be assigned as a value to variable 함수의 인자로 전달되고
// can be passed as an argument to other functions. 함수의 리턴값을 전달 가능
// cna be returned by another function

// 1. function expression
// a function declaration can be called earlier than it is defined. (hoisted)
// a function expression is created when the execution reaches it.
const print = function () { // anonymous function (함수 이름 없는 것)
    console.log('print');
};

const print = function print() { // named function (함수 이름 있는 것)
    console.log('print');
};
print();
const printAgain = print;
printAgain();
const sumAgain = sum;
console.log(sumAgain(1, 3));
// function declaration과 function expression의 가장 큰 차이는 function expression은 할당 후에 호출 가능, function declaration 선언 전에 호출 가능

// 2. calback function using functino expression
function randomQuiz(answer, printYes, printNo) { // 상황이 맞으면 함수를 전달해서 전달된 함수를 부르는 것
    if (answer === 'love you') {
        printYes();
    } else{
        printNo();
    }
}
// anonymous function
const printYes = function (){
    console.log('yes!');
}
// named function - 디버깅할 때 함수이름 보기 위해, 함수 안에서 또 다른 함수 호출할 때 ; 
const printNo = function print(){
    console.log('no!');
};
randomQuiz('wrong', printYes, printNo);
randomQuiz('love you', printYes, printNo);
// 함수 이름 있을 때랑 변수로 줄 때 호출 차이???

// Arrow functon
// always anonymous
const simplePrint = function () {
    console.log('simplePrint!');
};
const simplePrint = () => console.log('simplePrint!');
const add = (a, b) => a + b;

const simpleMultiply = (a, b) => {
    //do something more
    return a * b
}; // 단 블럭쓰면 return 키워드 통해 값 return 해야 함

// IIFE: Immediately Invoked Function Expression
(function hello() {
    console.log('IIFE');
})();

// FUn quiz time
// function calculate(command, a, b)
// command: add, substract, divide, mjltiply, remainder

function calculate(command, a, b) {
    switch (command){
        case 'add':
            console.log(a + b);
            break;
        case 'subtract':
            console.log(a - b);
            break;
        case 'divide':
            console.log(a / b);
            break;
        case 'multiply':
            console.log(a * b);
            break;
        case 'remainder':
            console.log(a % b);
            break;
    }
}
```

### class

더 연관있는 데이터를 묶어놓은 것으로 속성(fields)와 행동(methods)가 있다.
class를 통해 프로그램 내에서 상속과 다양화가 일어날 수 있다.  

#### 클래스

- 붕어빵을 만들 수 있는 틀
- template
- declare once
- no data in (메모리에 없음)

#### 오브젝트

- 실제 붕어빵
- instance of a class
- created many times
- data in

```jsx
'use strict';
// object-oriented programming
// class: template
// object: instance of a class
// JavaScript classes
// - introduced in ES6
// - syntactical sugar over prototype-based inheritance

// 1. Class declarations
class Person {
    // constructor
    constructor(name, age) {
    // fields
    this.name = name;
    this.age = age;
    }

    // methods
    speak() {
        console.log(`${this.name}: hello!`)
    }
}

// 새로운 object 만들 때 new를 사용
const ellie = new Person('ellie', 20);
console.log(ellie.name);
console.log(ellie.age);
ellie.speak();

// 2. Getter and setters 잘못된 설정에도 방어적으로 만들 수 있는 것
class User {
    constructor(firstName, lastName, age) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }

    get age() { // 값을 리턴
        return this._age;
    }

    set age(value) { // 값을 설정해야 해서 value가 있어야 함
        // if (value < 0) {
        //     throw Error('age can not be negative');
        // } 공격적인 것
        this._age = value < 0 ? 0 : value;
        // age라는 getter를 정의하는 순간 this.age는 getter를 호출
        // setter를 정의하는 순간 바로 메모리를 할당하는 것이 아닌 setter를 호출
        // 그런데 기존의 것과 getter, setter 이름이 같으면 setter 안에서 반복되어 call stack이 됨
        // 따라서 getter와 setter에는 이름을 다르게 해야 함
    }
}

const user1 = new User('steav', 'job', -1);
console.log(user1.age);

// 3. Fields (public, private)
// to soon!
class Experiment {
    pulicField = 2; // 외부 접근 가능
    #privateField = 0; // class 내부에서만 접근 가능하고 보여지고 변경 가능
}

const experiment = new Experiment();
console.log(experiment.pulicField);
console.log(experiment.privateField); // undefined

// 4. Static properties and methods
// to soon!
class Article {
    static publisher = 'subin'; // object에 상관없이 class가 가지고 있는 고유하며 반복적으로 사용되는 값
    constructor(articleNumber){
        this.articleNumber = articleNumber;
    }
    static printPublisher() {
        console.log(Article.publisher);
    }
}

const article1 = new Article(1);
const article2 = new Article(2);
console.log(Article.publisher); // class 자체에 할당
Article.printPublisher();

// 5. Inheritance (상속)
// a way for one class to extend another class.
class Shape {
    constructor(width, height, color) {
        this.width = width;
        this.height = height;
        this.color = color;
    }

    draw() {
        console.log(`drawing ${this.color} color!`);
    }

    getArea() {
        return this.width * this.height;
    }
}

class Rectangle extends Shape {} // shape에 있는 모든 것을 그대로 가져옴
class Triangle extends Shape { // 필요한 함수만 다시 재사용 overwrithing
    draw(){
        super.draw(); // overwrithing을 했을 때 부모의 method는 지워지는 데 부모까지 같이 표현하기 위해
        console.log('🔺');
    }
    getArea() {
        return (this.width * this.height) / 2;
    }
}

const rectangle = new Rectangle(20, 20, 'blue');
rectangle.draw();
console.log(rectangle.getArea());
const triangle = new Triangle(20, 20, 'red');
triangle.draw();
console.log(triangle.getArea());

// 6. Class checking: instanceOf
console.log(rectangle instanceof Rectangle); // T
console.log(triangle instanceof Rectangle); // F
console.log(triangle instanceof Triangle); // T
console.log(triangle instanceof Shape); // T
console.log(triangle instanceof Object); // T 모든 Object class는 Object 상속

// JS reference MDN 페이지 확인해보기
```
