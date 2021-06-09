
# 1. 스코프와 클로저 패턴

## 1-1. 실행 컨텍스트(scope)

실행 컨텍스트란 실행 가능한 코드가 실행되는 환경을 의미합니다.
JavaScript는 실행중인 코드가 실행 컨텍스트에 스택처럼 쌓이면서 실행 환경을 가지게 됩니다.
JavaScript는 **함수 스코프**를 따르기 때문에 함수를 호출할 때마다 함수 컨텍스트가 하나씩 생성됩니다.

```JavaScript

console.log("전역 컨텍스트입니다.");

function func1(){
    console.log("func1 입니다.");
}

function func2(){
    func1();
    console.log("func2 입니다.");
}
func2();

```

컨텍스트 생성 시 컨텍스트 내부에는 변수객체(arguments, variable), **스코프 체인**, this 가 생성됩니다.
컨텍스트 생성 후 함수가 실행되는데 사용되는 변수들은 변수 객체 안에서 값을 찾고, 없다면 스코프 체인을 따라 올라가며 찾습니다.
따라서 동일한 이름의 변수를 참조하여도 스코프 체인의 가장 앞에 존재하는 변수객체를 참조하기 때문에 참조하는 값이 달라질 수 있습니다.

```JavaScript

function foo(p1, p2, p3){
    var a;
    function bar(){
        // bar 변수객체에는 a가 없으므로 스코프 체인에 의해 foo 변수객체의 a 참조
        return a*a;
    }
  return p1 + p2 + p3 + a + bar();
}
foo(3, 4, 5);

```

- bar() 실행 컨텍스트 스코프 체인

[ bar 변수객체,
  foo 변수객체,
  전역 변수객체 ]

※ 스코프 체인 리스트의 제일 앞에 새롭게 생성된 변수객체 추가
※ this에 new나 bind를 통해 설정하지 않으면 기본적으로 window 객체

## 1-2. 어휘적 범위 지정(Lexical scoping)

어휘적 범위지정이란 자바스크립트가 변수의 유효범위를 지정하는 방법을 의미합니다.

```JavaScript

function init() {
  var name = "Mozilla"; // name은 init에 의해 생성된 지역 변수이다.

  function displayName() { // displayName() 은 내부 함수이며, 클로저다.
    alert(name); // 부모 함수에서 선언된 변수를 사용한다.
  }
  displayName();
}
init();

```

위의 상황을 보면 내부 함수 `displayName()` 이 외부 함수 `init()` 의 `name` 변수에 접근하고 있습니다.
이 예시를 통해 함수가 중첩된 상황에서 파서가 어떻게 변수를 처리하는지 알 수 있습니다.
즉, 어휘적 범위 지정은 변수의 스코프 범위를 알기 위해서 그 변수가 **호출되는 시점이 아니라 선언되는 시점**을 고려한다는 점을
알 수 있습니다. 다음의 예시를 통해 스코프는 변수의 **선언 시점**을 고려한다는 점을 확실하게 알 수 있습니다.

- 함수 선언 시점 비교
  - **함수 컨텍스트 단위**로 판단
  - **상위 요소**로 나아가면서 **선언된** 변수를 찾음(스코프 체인)

    ```JavaScript

    var name = 'zero';
    function log() {
        console.log(name); // 2) 전역 변수 name을 참조
    }

    function wrapper() {
        name = 'nero'; // 1) 전역변수 name 값 변경
        log();
    }
    wrapper(); // 결과 : nero 출력

    ```

    ```JavaScript

    var name = 'zero';
    function log() {
      console.log(name); // 2) 전역 변수 name을 참조
    }

    function wrapper() {
      var name = 'nero'; // 1) name 선언에 따른 새로운 스코프 생성(지역 변수 name)
      log();
    }
    wrapper(); // 결과 : zero 출력

    ```

## 1-3. 클로저 (Closure)

### 소개

클로저는 내부함수가 외부함수의 지역변수에 접근할 수 있는데 외부함수의 실행이 끝나서
외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근할 수 있는 매커니즘을 의미합니다.

```JavaScript

function makeFunc() {
  var name = "Mozilla";

  function displayName() { // Closure 형성
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc(); // myFunc변수에 displayName을 리턴
myFunc();

```

위의 예시를 보면 `makeFunc()` 의 실행이 끝나면 내부함수인 `displayName()` 함수는 `name` 변수에 접근할 수 없게
될 것으로 예상되지만, 실제로는 `name` 변수의 값을 통해 함수가 정상적으로 실행되는 모습을 볼 수 있습니다.
그 이유는  `var myFunc = makeFunc()` 를 실행하고 나면 스코프 체인은 Lexical Scoping을 따라 함수가
선언되는 시점인 `makeFunc()` 변수객체 와 전역 변수 객체를 포함하게 되기 때문입니다.
따라서 클로저 함수인 `myFunc()` 에서는 외부함수인 `makeFunc()`의 변수에 접근할 수 있습니다.

```JavaScript

"makeFunc 컨텍스트": {
  변수객체: {
    arguments: null,
    variable: [{ name: 'Mozilla' }],
  },
  scopeChain: ['makeFunc 변수객체', '전역 변수객체'],
  this: window,
}

```

```JavaScript

"myFunc 컨텍스트":  {
  변수객체: {
    arguments: null,
    variable: null,
  },
  scopeChain: ['myFunc 변수객체','makeFunc 변수객체', '전역 변수객체'],
  this: window,
}

```

### 장단점

장점

- 함수가 정의된 스코프 이외의 곳에서 사용될 때 관련 변수를 노출시키지 않아 private 처럼 사용해 외부의 접근을 차단할 수 있습니다.
  - 클로저를 사용한 카운터 예시

    ```JavaScript

    var counter = function() {
        var count = 0; // 외부에서 count 변수 접근 불가

        function changeCount(number) {
            count += number;
        }
        return {
            increase: function() {
                changeCount(1);
            },
            decrease: function() {
                changeCount(-1);
            },
            show: function() {
                alert(count);
            }
        }
    };
    var counterClosure = counter();
    counterClosure.increase();
    counterClosure.show(); // 1
    counterClosure.decrease();
    counterClosure.show(); // 0

    ```

- 클로저를 네임스페이스처럼 사용하여 전역변수의 사용을 억제하고 변수간의 충돌을 막을 수 있습니다.

단점

- 클로저로 인해 언제 관련 변수가 재사용될지 모르기 때문에 가비지 콜렉션을 통해 변수의 메모리 정리가 되지 않아 메모리 낭비로
  이어질 수 있습니다.
- 스코프 체인을 거슬러 올라가면서 변수를 찾기 때문에 성능이 떨어집니다.

### 사용 예시

- JS 커링
  - 함수가 일부 인자를 먼저 받고 나머지 인자를 나중에 받을 수 있는 패턴
  - 인자의 개수를 동적으로 받을 수 있음

    ```JavaScript
    
    const add = (x,y) => {
        if(y === undefined){
            return y => {
                return x + y;
            }
        }
        return x + y;
    }
    add(1,2) // 3
    add(3)(4) // 7
    // var add2 = add(3)
    // add2(4) 의 동작과 동일

    ```

- 클로저 패턴을 통한 JS 이벤트 핸들러
  - lexical scoping 에 따라 함수는 선언할 때 스코프가 생성되므로 내부 i는 5만 계속 참조

    ```JavaScript

    for (var i = 0; i < 5; i++) {
        target[i].addEventListener("click", function(){
            console.log(`This is Target ${i}`);
        });
    }

    ```

  - 클로저 패턴으로 해결 (각 target의 함수를 선언)

    ```JavaScript

      for (var i = 0; i < 5; i++) {
        (function(j) {
            target[j].addEventListener("click", function(){
                console.log(`This is Target ${j}`);
            });
            })(i);
      }

    ```

- useState
  - 클로저 패턴을 사용해 함수의 상태를 기억
  - 클로저를 통해 innerState값을 기억하기 때문에 이후에도 state에 접근 가능

  ```JavaScript
  const customUseState = (initialVal) => {
    let innerState = initialVal;

    const state = () => innerState;
    const setState = (newVal) => {
        innerState = newVal;
    };
    return [state, setState];
  }
  ```

- redux-thunk
  - redux-thunk는 액션생성자가 필요한 값을 파라미터로 받아 함수를 return하는 구조로 클로저 패턴 사용

    ```JavaScript

    // 액션 함수(액션 객체 return)
    const increment = gap => {
        return {
            type: INCREMENT_COUNTER,
            payload: { gap }
        };
    };

    const incrementAsync = (sec, gap) => dispatch => {
        setTimeout(() => {
            dispatch(increment(gap));
        }, sec);
    };

    // 위의 코드를 병합(클로저 패턴 사용 확인)
    function incrementAsync(sec, gap) {
        return function(dispatch) {
            setTimeout(() => {
                dispatch(increment(gap));
            }, sec);
        };
    }

    ```

<참고자료>

1. [MDN Docs - 클로저](https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures)
2. [ZeroCho 블로그 - 함수의 범위(scope)](https://www.zerocho.com/category/JavaScript/post/5740531574288ebc5f2ba97e)
3. [Chan BLOG - JS 실행 컨텍스트, 스코프(Scope), 스코프 체인](https://chanhuiseok.github.io/posts/js-4/)
4. [ZeroCho 블로그 -  실행 컨텍스트](https://www.zerocho.com/category/JavaScript/post/5741d96d094da4986bc950a0)
5. [project42da - 자바스크립트 커링(Currying)패턴과 생각](https://project42da.github.io/javascript/2019/02/06/js-curry.html)
6. [여울코딩 - 클로저와  useState Hooks](https://yeoulcoding.tistory.com/149)
7. [큐트리 개발 블로그 - Redux와 Redux 미들웨어 - thunk, saga](https://devsoyoung.github.io/posts/redux-middleware/)
