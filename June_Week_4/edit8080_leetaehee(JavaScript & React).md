
# 1. JavaScript 디바운싱과 쓰로틀링

기존에 JavaScript의 DOM 이벤트를 처리함에 있어서 과도한 이벤트 발생으로 인해 수많은 문제가 발생할 수 있었습니다.
예를 들어, 사용자의 입력에 따른 이벤트 발생 시 AJAX 요청을 전송한다면 무수히 많은 요청이 전송되는 문제점이 발생할 수 있고,
스크롤을 내릴 때마다 이벤트가 발생하여 스크롤 이벤트 처리가 어려워진다는 문제점 등이 있었습니다.
이러한 문제는 디바운싱과 쓰로틀링이란 기법이 등장하면서 해결할 수 있게 되었습니다.

디바운싱과 쓰로틀링은 이벤트의 발생을 제한하여 이벤트를 제어할 수 있는 수준으로 끌어내리는 기법입니다.
이러한 기법을 통해 DOM 이벤트를 최적화하여 자원과 성능을 향상시킬 수 있습니다. 디바운싱과 쓰로틀링은
타이머 (ex: `setTimeout()`)나 underscore, lodash 등 JavaScript 라이브러리의
`_.debounce`, `_throttle` 등을 통해 일정 시간 동안 발생한 이벤트를 하나로 그룹화하여 처리합니다.

## 1-1. 디바운싱(Debouncing)

디바운싱은 연이어서 발생하는 이벤트를 그룹화하여 특정시간이 지난 후 **마지막** 하나의 이벤트만 발생하도록 하는 기법입니다.
디바운싱을 사용하면 최종적인 결과가 중요한 이벤트 처리나 AJAX 요청을 수행할 때 자원과 성능을 최적화할 수 있습니다.

## 1-2. 쓰로틀링(Throttling)

쓰로틀링은 이벤트를 일정 주기마다 처리하고, 마지막 이벤트가 처리된 후 일정 시간이 지나기 전에 다시 처리되지 않도록 하는 기법입니다.
쓰로틀링은 일정한 시간 간격으로 호출되기 때문에 특정 간격마다 이벤트를 처리하고 싶을 때 주로 사용할 수 있습니다.
대표적인 예시로 무한 스크롤링 페이지가 있습니다. 스크롤을 할 때마다 footer에 도달하기 전에 콘텐츠를 가져와야 하기 때문에
쓰로틀링을 통해 이러한 작업을 진행할 수 있습니다. 또한 디바운싱보다 이벤트 처리를 더 빈번하게 수행함으로
훨씬 더 부드러운 UX 환경을 구성할 수 있습니다.

<참고 자료>

1. [Web Club - Throttle, Debounce & Difference](https://webclub.tistory.com/607)
2. [Zerocho - 쓰로틀링과 디바운싱](https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa)

# 2. React 디자인 패턴

## 2-1. MVC 패턴

MVC 패턴은 Model, View, Controller의 약자로 하나의 어플리케이션을 구성할 때 구성요소를 세 가지의 역할로 구분한
패턴입니다. MVC 패턴은 사용자나 내부에서 발생한 동작(action)이 Controller에 전달되면 action에 따라 Model을
업데이트하고 View를 통해 변경된 Model을 사용자에게 제공되는 형태로 구성됩니다.
MVC 패턴은 Model과 View 사이에서 양방향 데이터 바인딩을 구성하여 데이터를 송수신합니다.

> <양방향 데이터 바인딩>
>
> 사용자가 View에서 발생시킨 동작을 통해 Model 을 변경할 수 있고, 내부의 동작에 의해  변경된 Model이 View에 반영된다.

- Model : 어플리케이션의 정보, 데이터
- View : 사용자 인터페이스
- Controller : 사용자나 내부에서 발생한 동작 처리

MVC 패턴은 간단하면서도 보편적으로 사용할 수 있는 디자인 패턴이지만, 어플리케이션의 규모가 거대해질수록 Model과 View간에
양방향 데이터 바인딩 구조에 의해 의존성이 높아져 데이터 흐름이 복잡해지고 유지보수가 어려워질 수 있습니다.
또한 MVC 패턴은 하나의 Controller가 여러 Model과 연결될 수 있고, 여러 Model이 여러 View와 연결되어 있을 수
있습니다. 이러한 구조는 데이터가 신속하게 전파되는 것을 막고 성능이 저하되는 문제가 발생할 수 있습니다.

## 2-2. Flux 패턴

Flux 패턴은 페이스북에서 위에서 설명한 MVC 패턴의 단점을 해결하기 위해 만든 디자인 패턴입니다.
Flux 패턴은 Model과 View간에 구성된 양방향 데이터 바인딩에서 벗어나 단방향으로만 데이터를 변경할 수 있도록 구성되어있습니다.

> <단방향 데이터 바인딩>
>
> 사용자가 View에서 발생시킨 동작이나 내부에서 발생한 동작 모두 Dispatcher에 의해 적합한 동작을 실행하고 해당 결과를
   **적합한(복수)** Store에 저장한다. (View에서 Store에 바로 접근할 수 없음)

- Dispatcher : Action에 맞는 데이터 흐름 관리
- Store : 어플리케이션의 상태와 상태 변경 메소드를 가지고 있는 저장소
- View : 사용자 인터페이스

Flux 패턴은 단방향 데이터 바인딩에 의해 데이터(Store)와 표현 계층(View) 간의 복잡성을 확연히 감소시켰습니다.
또한 어플리케이션 규모가 커질 때 기존 MVC 패턴에서는  여러 개의 Model과 View가 구성되면서 의존성이 높아졌던 반면,
Flux 패턴은 새로운 기능이 다른 Model에 영향을 끼치는 것을 고려하지 않고 해당 기능의 액션에 맞는 dispatcher 기능을
개발하는 것에만 집중하면 되기 때문에 컴포넌트의 독립성을 높일 수 있습니다.

### Flux 패턴과 Redux

Flux 패턴을 도입한 대표적인 상태 관리 라이브러리로 Redux가 손꼽히고 있습니다. 하지만 Redux는 Flux 패턴을 완벽하게
도입하진 않았습니다. Redux는 Flux 패턴 이외에 함수형 프로그래밍, 불변성 등을 혼합하여 만들었기 때문에 순수 Flux 패턴과
약간의 거리가 있습니다. 대표적인 예로, Redux는 Dispatcher의 개념이 없고 Reducer가 Dispatcher와 부분적
Store의 기능을 담당하고 있습니다. 또한 Redux의 Store는 싱글톤 패턴을 따르기 때문에 하나의 어플리케이션에는
하나의 스토어만 사용할 수 있습니다.

## 2-3. Presentational & Container 디자인 패턴

Presentional & Container 패턴은 컴포넌트의 재사용성과 유지보수성을 높이기 위해 사용하는 디자인 패턴입니다.
Presentional 컴포넌트가 화면에 렌더링하기 위한 UI 컴포넌트라면, Container 컴포넌트는 Presentional 컴포넌트의
상태를 관리하고 어떻게 동작할지 처리하는 컴포넌트입니다.

UI 컴포넌트와 해당 컴포넌트의 동작을 처리하는 컴포넌트를 따로 구분하면, UI 컴포넌트는 DOM 마크업과 스타일에만 집중해서
작업할 수 있고 다른 영역에서 편리하게 재사용할 수 있습니다. 또한 Container 컴포넌트에서만 상태를 관리하기 때문에
유지보수가 용이하다는 장점이 있습니다.

<참고자료>

1. [버미노트 - [디자인패턴] MVC, MVP, MVVM 비교](https://beomy.tistory.com/43?category=676748)
2. [huskyhoochu Blog - React의 flux 패턴](https://www.huskyhoochu.com/flux-architecture/)
3. [bestalign Blog - Flux로의 카툰 안내서](https://bestalign.github.io/cartoon-guide-to-flux/)
4. [jeffgukang Blog - Container 컴포넌트와 Presentational 컴포넌트](https://jeffgukang.github.io/react-native-tutorial/docs/state-tutorial/redux-tutorial/04-container-and-presentational/container-and-presentational-kr.html)
