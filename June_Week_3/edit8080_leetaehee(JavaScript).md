
# 1. JavaScript의 이벤트 위임(delegation)

## 1-1. 이벤트 버블링

이벤트 버블링이란 요소에 할당된 이벤트 핸들러가 동작하면 최상단의 부모 요소를 만날때까지 이벤트가 전파되어 각각에 할당된 핸들러가
동작하는 것을 의미합니다. 이벤트가 상위 요소까지 전파되는것을 중단하고 싶다면 해당 요소에서 `event.stopPropagation()`
메서드를 활용합니다. 또한 이벤트가 정확히 어디서부터 시작되었는지 확인하기 위해서는 `event.target`을 활용합니다.

## 1-2. 이벤트 캡처링

이벤트 캡쳐링은 이벤트가 상위 요소로 전파되는 이벤트 버블링과 반대로 이벤트가 하위 요소로 전파되는 것을 의미합니다.
특정 이벤트가 발생했을 때 최상위 요소인에서 `Window` 객체에서부터 출발해 해당 이벤트가 발생한 요소까지 해당 이벤트를 전파합니다.
이벤트 캡쳐링은 `addEventListener()` 함수에서 옵션 객체에 `capture:true` 를 설정하는 것으로 사용할 수 있습니다.
이외에도 `addEventListener(..., true)` 와 같이 축약하여 사용할 수도 있습니다.
이벤트 캡처링은 이벤트 버블링과 마찬가지로 `event.stopPropagation()` 메소드를 활용해 이벤트 전파를 방지할 수 있습니다.

## 1-3. JavaScript 이벤트 위임(delegation)

이벤트 위임은 모든 DOM 요소에 이벤트 핸들러를 개별적으로 할당하지 않고 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당해도
여러 DOM 요소의 이벤트를 한꺼번에 다룰 수 있는 기법입니다. 이벤트 위임은 공통 조상에 할당한 이벤트 핸들러에서 `event.target`
과 이벤트 버블링, 캡쳐링 기법을 활용하는 것으로 구현할 수 있습니다.

### 장점

- 많은 핸들러를 할당하지 않아도 되기 때문에 초기화가 단순해지고 메모리가 절약됨
- 요소를 추가하거나 제거할 때 해당 요소에 할당된 핸들러를 추가하거나 제거할 필요가 없기 때문에 코드가 짧아짐
- `innerHTML` 등으로 DOM 요소를 쉽게 추가하거나 제거할 수 있어서 DOM 수정이 용이함

### 단점

- 이벤트 위임을 사용하려면 이벤트가 반드시 전파되어야함
- 모든 하위 컨테이너에 대해 이벤트를 담당하는 조상 요소가 응답해야하므로 CPU의 작업 부하가 늘어날 수 있음(무시할만한 수준)

<참고 자료>

1. [모던 자바스크립트 - 버블링과 캡처링](https://ko.javascript.info/bubbling-and-capturing)
2. [모던 자바스크립트 - 이벤트 위임](https://ko.javascript.info/event-delegation)
3. [캡틴 판교 블로그 - 이벤트 버블링, 이벤트 캡쳐 그리고 이벤트 위임까지](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/#%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%BA%A1%EC%B3%90---event-capture)

# 2. JavaScript의 프로토타입

## 2-1. JavaScript의 프로토타입 이란?

JavaScript에는 클래스라는 개념이 존재하지 않아 대신 기존의 객체를 복사하여 새로운 객체를 생성하는 프로토타입 개념을 사용합니다.
JavaScript는 프로토타입을 이용하여 객체를 확장하고 객체 지향적인 프로그래밍을 수행할 수 있습니다.

프로토타입은 두 가지 형태로 해석할 수 있습니다. 하나는 프로토타입 객체를 참조하는 `prototype` 속성과
다른 하나는 객체 멤버인 `__proto__`  속성이 참조하는 숨은 링크입니다.

`prototype` 속성은 객체를 생성할 때 자신이 다른 객체의 원형이 되는 프로토타입 객체를 가리키기 위한 속성입니다.
프로토타입 객체는 원본 객체를 복사하여 생성된 새로운 객체이므로 프로토타입 객체에서 선언된 값이나 함수는 원본 객체에서 접근할 수 없습니다.

```JavaScript

function Person(){}

// new 생성자를 사용하면 프로토타입 객체(Person.prototype)가 복사
let joon = new Person();
let jisoo = new Person();

// Person.prototype : Person을 복사한 프로토타입 객체
// 서로 다른 객체이므로 Person.getType()은 에러가 발생
Person.prototype.getType = function (){ 
    return "인간"; 
};

console.log(joon.getType());   // 인간

```

`__proto__` 속성은 프로토타입 객체를 참조하기 위한 getter, setter 입니다. `__proto__` 속성을 이용해
프로토타입을 획득하거나 설정할 수 있습니다. 단, `__proto__` 속성을 통한 순환 구조는 허용되지 않고
`__proto__` 속성값은 객체나 null 만 가능하다는 제약 조건이 존재합니다.

```JavaScript

let animal = {
  eats: true,
  walk() {
    alert("동물이 걷습니다.");
  }
};

let rabbit = {
  jumps: true,
  __proto__: animal // 프로토타입 객체 참조
};

rabbit.walk(); // animal.walk() 수행

```

## 2-2. 프로토타입 상속

프로토타입 객체는 상위 객체로부터 메소드와 속성을 상속받을 수 있고 그 상위 객체도 동일한 동작을 수행할 수 있습니다.
이를 프로토타입 체인이라 부르며 프로토타입 체인을 이용하면 다른 객체에 정의된 메소드와 속성을 한 객체에서 사용할 수 있습니다.

이와 같이 프로토타입 체인으로 연결된 프로퍼티를 참조하려고 할 때 원본 객체를 통해 동일한 프로퍼티에 접근하는 것과는
큰 성능 차이가 없습니다. JS 엔진은 프로퍼티를 발견할 때 발견한 위치를 내부적으로 기억하고 있다가 요청이 발생했을 때
해당 정보를 재사용하여 프로토타입을 최적화하고 있기 때문입니다.

```JavaScript

let animal = {
  eats: true,
  walk() {
    alert("동물이 걷습니다.");
  }
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

let longEar = {
  earLength: 10,
  __proto__: rabbit
};

// 메서드 walk는 프로토타입 체인을 통해 상속받음
longEar.walk();
alert(longEar.jumps);

```

### this 객체

`this`는 프로토타입에 영향을 받지 않습니다. 메소드를 객체에서 호출하거나 프로토타입에서 호출한 것과 상관없이 `this` 는
언제나 `.` 앞에 있는 객체가 됩니다. `this` 가 가리키는 객체에서 값을 변경하려는 프로퍼티가 없다면 프로토타입 체인을 거슬러
올라가면서 관련있는 프로퍼티를 찾고, 최상위 프로토타입 객체에도 존재하지 않는다면 `this` 가 가리키는 객체에 프로퍼티를
새롭게 할당합니다.

```JavaScript

let animal = {
  walk() {
    if (!this.isSleeping) {
      alert(`동물이 걸어갑니다.`);
    }
  },
  sleep() {
    this.isSleeping = true;
  }
};

let rabbit = {
  name: "하얀 토끼",
  __proto__: animal
};

// this는 rabbit을 가리키지만 isSleeping 프로퍼티가 없고 상위 프로토타입에도 isSleeping 프로퍼티가 없음
// rabbit의 프로퍼티에 isSleeping:true를 새롭게 할당
rabbit.sleep();

alert(rabbit.isSleeping); // true
alert(animal.isSleeping); // undefined (프로토타입에는 isSleeping 프로퍼티가 없음)

```

## 3. __proto__ 속성의 대체

`__proto__` 속성은 ES5 까지는 비표준이었지만 ES6 부터 표준으로 자리잡아 `__proto__` 속성을 통한 프로토타입 지정은
문제 없이 동작하고 있습니다. ES6 부터 이를 표준으로 지정한 이유는 이미 이전까지 `__proto__` 를 통해 프로토타입을
지정한 경우가 많았기 때문에 호환성 차원에서  `__proto__` 속성을 표준으로 지정하였습니다. 하지만 여전히 `__proto__` 속성을
통해 값을 직접적으로 대입하는 것은 추천하지 않는 방식입니다. 현재 이를 보완하기 위해 아래와 같은 메소드를 지원하고 있습니다.

- `Object.create(proto, [descriptors])` –  `proto`를 참조하는 빈 객체를 만듭니다.
이때 프로퍼티 설명자를 추가로 넘길 수 있습니다.
- `Object.getPrototypeOf(obj)` – `obj`의 프로토타입을 반환합니다.
- `Object.setPrototypeOf(obj, proto)` – `obj`의  프로토타입이 `proto`가 되도록 설정합니다.

```JavaScript

let animal = {
  eats: true
};

// 프로토타입이 animal인 새로운 객체를 생성
let rabbit = Object.create(animal);
alert(rabbit.eats); // true
// 프로토타입
alert(Object.getPrototypeOf(rabbit) === animal); // true

let horse = {
    run: true
}
// horse의 프로토타입을 animal로 설정
Object.setPrototypeOf(horse, animal); 

```

<참고 자료>

1. [MDN Docs - Object prototypes](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Object_prototypes)
2. [nextree blog - JavaScript: 프로토타입(prototype) 이해](https://www.nextree.co.kr/p7323/)
3. [모던 자바스크립트 - 프로토타입 상속](https://ko.javascript.info/prototype-inheritance)
4. [MDN Docs - Object.prototype.__proto__](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)
5. [모던 자바스크립트 - 프로토타입 메서드와 __proto가 없는 객체](https://ko.javascript.info/prototype-methods)
