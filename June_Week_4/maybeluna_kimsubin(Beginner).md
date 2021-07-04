# Javascript 다시 공부

## Javascript

### array

- 비슷한 종류를 다 담아놓는 것을 자료구조
- 비슷한 타입의 자료구조를 묶어넣는 것으로 다양한 타입만 묶을 수 있지만 되도록 동일 타입 추천


```jsx
'use strict';

// 1. declaration
const arr1 = new Array();
const arr2 = [1, 2];

// 2. Index postion
const fruits = ['🍎', '🍉'];
console.log(fruits);
console.log(fruits.length);
console.log(fruits[0]);
console.log(fruits[1]);
console.log(fruits[2]);
console.log(fruits[fruits.length - 1]);

// 3. Looping over an array
// print all fruits
// a. for
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}
// b. for of
for (let fruit of fruits) {
    console.log(fruit);
}
// c. forEach
fruits.forEach(function (fruit, index, array){
    console.log(fruit, index, array);
});

fruits.forEach((fruit, index) => { // 함수 이름 없는 경우 arrow function 가능
    console.log(fruit, index);
});

fruits.forEach((fruit, index) => console.log(fruit, index)); // arrow function 한 줄인 경우 {} 없앨 수 있음

// 4. Addition, deletion, copy
// push: add an item to the end
fruits.push('🍓', '🍒');
console.log(fruits);

// pop: remove an item from the end
fruits.pop();
fruits.pop()
console.log(fruits);

// unshift: add an timen to the beginning
fruits.push('🍌', '🍑');
console.log(fruits);

// shift: remove an item from the beginnig
fruits.shift();
fruits.shift();
console.log(fruits);
// note!! shift, unshift are slower than pop, push
// 전체 데이터가 움직여야하는 경우 느리다! -> 빅오

// splice: remove an item by index position
fruits.push('🍍', '🍋', '🍈');
console.log(fruits);
fruits.splice(3); // start 점 이후 아무것도 지정 안하면 start 지점부터 뒤는 싹 다 없어짐
console.log(fruits);
fruits.push('🍍', '🍋', '🍈');
fruits.splice(1, 2);
fruits.splice(1, 0, '🍏', '🍐'); // 데이터를 지우지는 않고 넣기만 가능
fruits.splice(2, 1, '🥑', '🍇'); // splice로 지우고 그 자리에 추가도 됨 
console.log(fruits);

// combine two arrays
const fruits2 = ['🥝', '🥭'];
const newFruits =  fruits.concat(fruits2); // 서로 array 합치는 것
console.log(newFruits);

// 5. Searching
// indexOf: find the index
console.clear();
console.log(fruits.indexOf('🍎')); // 해당 값 없으면 -1
console.log(fruits.indexOf('🍍')); // 해당 값의 인덱스 출력
console.log(fruits.indexOf('🥥'));

// include
console.log(fruits.includes('🥥')); // 해당값 유무
console.log(fruits.includes('🍇'));

//lastIndexOf
console.clear();
fruits.push('🍍');
console.log(fruits);
console.log(fruits.indexOf('🍍')); // 중복된 값 중 가장 처음에 있는 것
console.log(fruits.lastIndexOf('🍍')); // 중복된 값 중 가장 끝에 있는 것
```

### event

event란 마우스 올리기, 클릭하기 등 웹 상에서 일어나는
모든 활동을 **event**라고 한다.

```jsx
// css 변경
console.log(title);
console.dir(title); // 내부를 보고 싶을 때

title.innerText = "hoy";

title.style.color = "blue"; // h1의 style을 JS로 변경할 수 있는 지 확인

// event listen
const title = document.querySelector("div.hey:first-child h1");

// event란 클릭, 마우스 올리기, 입력 끝내거나, 내 이름 적거나, 엔터를 하거나 등
// 모든 것을 event라고 함!

function handleTitleClick() {
    console.log("title was clicked");
    title.style.color = "blue"; // JS도 style를 변경할 수 있지만 css로 변경하는 것이 옳은 것
    
}
// event를 listen하는 것
title.addEventListener("click", handleTitleClick); 
// click 하면 function 실행
// event 발생 시 JS가 function 실행버튼 대신 눌러 주는 것
// function은 ()를 추가해야 실행버튼을 누르는 것 

// HTML element.addEventListner("event", function);

function handleMouseEnter(){
    title.innerText = "Mouse is here";
}
title.addEventListener("mouseenter", handleMouseEnter); 
// mouse h1 올리면 function 실행

function handleMouseLeave() {
    title.innerText = "Mouse is gone";
}
title.addEventListener("mouseleave", handleMouseLeave); 
// 마우스 h1 밖에 있으면 function 실행

// 다양한 event
const h1 = document.querySelector("div.hey:first-child h1");

function handleTitleClick() {
    console.log("title was clicked");
    h1.style.color = "blue"; // JS도 style를 변경할 수 있지만 css로 변경하는 것이 옳은 것   
}

function handleMouseEnter(){
    h1.innerText = "Mouse is here";
}

function handleMouseLeave() {
    h1.innerText = "Mouse is gone";
}

function handleWindowResize() {
    document.body.style.background = "tomato";
}

function handleWindowCopy(){
    alert("copier!")
}

function handleWindowOffline(){
    alert("SOS no WIFI")
}

function handleWindowOnline(){
    alert("ALL GOOD")
}

h1.onclick = handleTitleClick; // event 실행하는 다른 방법
h1.onmouseenter = handleMouseEnter;
h1.addEventListener("mouseleave", handleMouseLeave); 
// .removeEventListener로 addEventListener은 onclick과 달리 이렇게 삭제가 가능함
window.addEventListener("resize", handleWindowResize);
window.addEventListener("copy", handleWindowCopy);
window.addEventListener("offline", handleWindowOffline);
window.addEventListener("online", handleWindowOnline);

// document.body 등과 같이 중요한 태그인 body, head, title은 존재하지만
// 나머지 element는 querySelector나 getElementById 등으로 찾아와야 함
```

#### listen할 event를 찾는 좋은 방법

1. 구글링 `MDN HTML HeadingElement Web APIs`에서 찾아보기
2. console.dir(element)를 통해서 사용가능한 event 찾아보기 (**on-**이 event listner)  
