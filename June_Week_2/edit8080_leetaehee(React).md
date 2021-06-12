
# 1. React의 생명주기

## 1-1. React 생명주기와 생명주기 메소드

모든 React 컴포넌트는 생성→ 변경 → 제거의 생명주기를 가지고 있습니다. 이 생명주기를 따라서 컴포넌트가
생성되거나 변경될 때 해당 동작과 관련된 특정 이벤트가 발생합니다. 이러한 이벤트가 발생했을 때 생명 주기
메소드를 통해 특정 동작을 수행하거나 조건을 걸어 렌더링을 하는 등의 동작을 수행할 수 있습니다.

- 생명주기 메소드
- 컴포넌트 생성(mount)
  - `constructor()` : 컴포넌트가 생성될 때 가장 먼저 실행
  - `getDerivedStateFromProps()` : props로 받아온 값들을 state에 동기화하기 위해 사용
  - `render()` : 컴포넌트를 렌더링
  - `componentDidMount()` : 컴포넌트 렌더링 완료 후 호출

- 컴포넌트 변경(update)
  - `getDerivedStateFromProps()` : props로 받아온 값들을 state에 동기화
  - `shouldComponentUpdate()` : 리렌더링을 수행할 조건 지정
  - `render()` : 컴포넌트를 렌더링
  - `getSnapshotBeforeUpdate()` : 렌더링한 컴포넌트를 DOM 에 연결하기 직전에 접근
  - `componentDidUpdate()` : 컴포넌트 업데이트

- 컴포넌트 제거(unmount)
  - `componentWillUnmount()` : 컴포넌트가 사라지기 전에 호출

## 1-2. 함수형 컴포넌트(useEffect)

클래스 컴포넌트 때는 라이프사이클이 컴포넌트에 맞춰져 있어 클래스를 통해 컴포넌트가 생성, 변경, 제거될 때
생명주기 메소드가 실행되었습니다. 하지만 함수형 컴포넌트는 **컴포넌트와 데이터**를 기준으로 생명주기가 진행됩니다.
즉,  컴포넌트가 생성되고, 데이터가 변경되고, 컴포넌트가 제거될 때 생명주기가 진행됩니다. 이러한 생명주기를 활용하기
위해 함수형 컴포넌트에서는 `useEffect()` 라는 Hooks를 활용합니다. `useEffect()` 함수에서는 의존성 배열을 활용해
기준으로 삼을 데이터를 지정하고(=`componentDidUpdate()`), return을 활용해 컴포넌트가
제거될 때(=`componentWillUnmount()`) 수행할 동작을 실행합니다.

```jsx

useEffect(() => {
  // 컴포넌트가 생성(mount)될 때와 hidden이 변경(update)될 때 실행됩니다.
  console.log('hidden changed');

  // 컴포넌트가 제거(unmount)될 때 실행됩니다.
  return () => {
    console.log('hidden이 바뀔 예정입니다.');
  };
}, [hidden]); // 변경을 탐지할 데이터를 설정합니다.(의존성 배열)

```

※ 의존성 배열의 활용

- 마운트 될 때 처음 한 번만 실행하고 싶은 경우
  - 의존성 배열에 빈 배열만 넣어줌

      ```jsx

      useEffect(() => {
        console.log('mounted');
        return () => {
          console.log('unmount');
        }
      }, []);

      ```

- 컴포넌트가 리렌더링 될 때마다 실행하고 싶은 경우
  - 의존성 배열을 아예 명시하지 않음

      ```jsx

      useEffect(() => {
        console.log('rerendered!');
      });

      ```

<참고자료>

1. [React Docs - React.Component](https://ko.reactjs.org/docs/react-component.html)
2. [벨로퍼트 모던 리액트 - LifeCycle Method](https://react.vlpt.us/basic/25-lifecycle.html)
3. [Zerocho blog - React Hooks! useEffect편(React 17버전)](https://www.zerocho.com/category/React/post/5f9a6ef507be1d0004347305)

# 2. JavaScript의 이벤트 루프

## 2-1. 자바 스크립트 엔진의 구성

자바스크립트 엔진은 자바스크립트로 작성한 코드를 해석하고 실행하는 인터프리터입니다.
대부분의 자바스크립트 엔진은 크게 호출스택(Call Stack), 작업 큐(Event Queue), 힙 영역으로 구성되어있습니다.

### 호출 스택(Call Stack)

자바스크립트는 단 하나의 호출 스택을 사용합니다. 따라서 하나의 함수가 실행되면 이 함수의 실행이 끝날 때까지 다른 어떠한 작업도 수행될
수 없습니다. 함수가 실행될 때 호출 스택에 해당 함수를 push한 후 함수 실행이 종료되면 호출 스택에서 해당 함수를 pop 하게 됩니다.

```jsx

// 1. 전역 함수(main) push

function first() {
  second(); // 3. second() 함수 push
  console.log('첫 번째');
}
function second() {
  third(); // 4. third() 함수 push
  console.log('두 번째');
}
function third() {
  console.log('세 번째');
}
first(); // 2. first() 함수 push

```

### 힙(Heap)

자바스크립트에서 생성된 객체는 힙 영역에 할당됩니다. 힙은 구조화되지 않은 넓은 메모리 영역을 의미합니다.

### 작업 큐(Event Queue)

자바스크립트의 런타임 환경에서는 작업 큐에 처리해야하는 작업들을 임시로 저장합니다. 이후 호출 스택이 **비어진 다음** 작업 큐에
대기중인 순서대로 작업을 수행합니다.

자바스크립트에서 **비동기**로 실행되는 함수들은 호출 스택에 push되지 않고 작업 큐에 push 됩니다.
JavaScript의 이벤트나 Web API 함수들이 주로 비동기적으로 실행됩니다.

```jsx

function test1(){
    console.log("test1");
    test2(); // 2. test2를 호출 스택에 push
}
function test2(){
    setTimeout(function(){ // 3. setTimeout()을 호출 스택에 push
        console.log("test2"); // 4. 익명 함수를 작업 큐에 push
    },0); // 5. setTimeout() 을 pop
    test3(); // 6. test3를 호출 스택에 push
}
function test3(){
    console.log("test3");
}
test1(); // 1. test1을 호출 스택에 push

```

위의 코드의 동작 순서를 살펴보면 test1이 먼저 호출 스택에 push하고 test2를 호출합니다. 호출된 test2를 호출 스택에
push하고 `setTimeout()` 함수를 실행합니다. setTimeout() 함수는 호출 스택에 push되고 바로 pop되지만 내부의
익명함수는 비동기적 처리를 위해 작업 큐에 push합니다. 이후 test3도 호출 스택에 push된 후 호출 스택에 저장된
모든 작업을 수행하여 호출 스택이 비어 있게 된 순간 작업 큐에 저장된 익명 함수를 가져와 실행하게됩니다.

## 2-2. 이벤트 루프

**이벤트 루프는 자바스크립트 엔진에서 작업 큐에 들어가는 작업들을 관리**하는 역할을 담당합니다. 이벤트 루프는 루프를 돌면서
호출 스택과 작업 큐를 확인합니다. 그러다 호출 스택이 비게되면 작업 큐에 있는 함수를 하나씩 호출 스택으로 이동시킵니다.
이러한 이벤트 루프의 특성 때문에 자바스크립트를 **싱글 스레드**라고 부릅니다.

```jsx

// 이벤트 루프의 대략적 구조
while(queue.waitForMessage()){ // 큐에 대기하는 작업이 있다면
  queue.processNextMessage(); // 가장 앞에 있는 작업을 실행
}

```

```jsx

function run() {
  console.log('동작');
}
console.log('시작');
setTimeout(run, 3000);
console.log('끝');

```

위의 코드 동작을 살펴보면 `console.log('시작')` 이 바로 호출 스택에 push되었다가 pop이 되고
`run` 함수(콜백 함수)와 함께 3초 타이머를 백그라운드로 전송합니다. 백그라운드는 3초의 시간이 지나면 작업 큐에
`run` 함수를 전송합니다. 이러한 백그라운드 환경은 브라우저나 node를 통한 런타임 환경을 통해 구성됩니다.
외부 런타임 환경을 통해 자바스크립트는 싱글 스레드이지만 **멀티스레드**로 작업을 처리할 수 있습니다.

작업 큐를 관리하는 이벤트 루프는 호출 스택이 비게 될 때까지 대기하고 있다가 `setTimeout()` 과
`console.log('끝')` 의 실행이 종료되어 호출 스택이 비게 된다면 작업 큐에 대기 중인 `run` 함수를 호출 스택으로
전송합니다. 이 과정에서 백그라운드에서 3초를 센 후 `run` 함수를 이벤트 큐로 전송한다고 해도 호출 스택이 빌 때까지
대기해야하기 때문에 실제 실행에서는 3초 보다 긴 시간이 지난 다음 실행될 수도 있습니다.

<참고 자료>

1. [_Ibee blog - JavaScript의 Event Loop](https://asfirstalways.tistory.com/362)
2. [ZeroCho blog - 호출 스택과 이벤트루프](https://www.zerocho.com/category/JavaScript/post/597f34bbb428530018e8e6e2)
3. [MDN Docs - 동시성 모델과 이벤트 루프](https://developer.mozilla.org/ko/docs/Web/JavaScript/EventLoop)
