
# 1. DOM과 Virtual DOM의 기능과 구조

## 1-1. DOM

DOM은 문서 객체 모델(Document Object Model)의 약자로 HTML, XML 문서의 프로그래밍 인터페이스 입니다.
DOM은 구조화된 nodes, property, method를 갖고 있는 객체 형태로 웹 페이지 문서를 표현합니다.
따라서 DOM은 **웹 페이지에 대한 객체 지향 표현**이라고 할 수 있으며 JavaScript에서 DOM API를 사용해
DOM을 수정할 수 있습니다.

웹 브라우저가 웹 페이지를 렌더링하는 과정은 문서의 구조인 DOM과 문서에 적용할 스타일을 나타낸 CSSOM (CSS Object Model)
을 통해 렌더 트리를 구축하여 적용하는 방식으로 이루어집니다. 이와 같이 렌더링은 HTML 을 통한 DOM과 CSS 를 통한 CSSOM이
따로 적용되기 때문에 CSS를 통한 요소 삽입이나(ex : 가상 요소 `::before`) 요소를 감추는
동작(ex : `display: none`) 은 **렌더 트리에는 영향을 미치지만 DOM에는 어떠한 영향도 끼치지 않고** 반대로 DOM을
통해 CSS의 가상 요소들을 조작할 수 없습니다. 또한 렌더링을 위해 구축한 DOM은 HTML 문서와 1:1로 대응되는 것처럼 보이지만
아래의 두가지 예시를 살펴보면 **DOM과 HTML은 항상 동일하지 않다**는 사실을 알 수 있습니다.

1. 작성된 HTML 문서가 유효하지 않더라도 DOM 내에선 올바르게 교정되어 나타남
2. JavaScript에 의해 DOM 이 수정될 경우 HTML 문서의 내용을 변경하진 않지만 DOM에 새로운 노드를 추가할 수 있음

## 1-2. Virtual DOM

### 등장 배경

Virtual DOM은 **DOM의 비효율적인 렌더링을 보완**하기 위해 등장한 개념입니다. 기존의 DOM은 각 노드에 대한 수정이 가해질
때마다 전체 HTML 문서를 다시 파싱하여 DOM 트리를 만들고 그에 따른 렌더 트리를 만들어 렌더링을 수행하였습니다.
이러한 문제를 해결하기 위해 Virtual DOM 은 **모든 노드 변화를 묶어 한번에 실제 DOM에 적용하는 기법**을 사용합니다.
이 기법을 통해 리렌더링의 횟수를 줄여 성능을 향상시킵니다.

### 비교(Diffing) 알고리즘

Virtual DOM 을 통해 모든 노드 변화를 묶어 실제 DOM에 적용하는 기법을 활용하기 위해서는 기존의 DOM에서 어떠한 변화가
발생했는지를 탐지해야합니다. Virtual DOM은 이 탐지를 수행하기 위해 비교(Diffing) 알고리즘을 사용합니다.
하나의 트리를 가지고 다른 트리로 변환하기 위한 최소한의 연산 수를 구하는 최신의 알고리즘도 O(n^3)의 복잡도를 가지기 때문에
React는 두 가지 가정에 기반하여 O(n)복잡도를 가지는 비교 알고리즘을 구현하였습니다.

1. 서로 다른 타입의 두 엘리먼트는 자식 노드의 비교 없이 서로 다른 트리를 만들어낸다.
2. `key` props를 통해 여러 렌더링에서 어떤 자식 엘리먼트가 변경되지 않아야하는지 표시한다.

- 엘리먼트 타입이 다른 경우 (하위 요소 비교없이 삭제 후 마운트)
    1. 이전 DOM 트리 파괴(자식 모두 탐색 없이 리렌더링), 컴포넌트는 `componentWillUnmount()` 호출
    2. 새로운 DOM 트리를 구축하여 마운트- `componentDidMount()` 호출

    ```jsx

    <!--기존 엘리먼트-->
    <div>
      <Counter />
    </div>

    <!--엘리먼트 타입이 다름 → Counter 컴포넌트가 새로 렌더링 -->
    <span>
      <Counter />
    </span>

    ```

- 엘리먼트 타입이 같은 경우 (삽입, 삭제 없이 속성값만 변경)
  1. 두 엘리먼트의 속성을 확인하여 동일한 속성은 유지, 변경된 속성만 갱신. 이후 해당 노드의 자식들을 재귀적으로 처리
  2. 컴포넌트의 속성이 갱신되면 컴포넌트의 state는 유지되고 props를 갱신(컴포넌트의 `componentDidUpdate()`  호출)
  3. 이후 컴포넌트는 `render()` 를 호출하여 이전 결과와 새로운 결과를 재귀적으로 처리

  ```jsx

  <div className="before" title="stuff" />

  <!--엘리먼트 타입이 같음 → className만 수정-->
  <div className="after" title="stuff" />

  ```

- 효율적인 자식 재귀 호출 (삭제, 변경없이 새로운 노드 추가)
    1. 엘리먼트의 효율적인 비교를 위해 `key` 속성을 기반으로 비교
    2. `key` 속성은 전역적으로 유일할 필요는 없고 형제 노드 사이에서만 유일하면 됨

    ```jsx

    <ul>
      <li key="2015">Duke</li>
      <li key="2016">Villanova</li>
    </ul>

    <!--key 속성을 통해 2014 엘리먼트는 추가 / 2015,2016 엘리먼트는 이동, -->
    <ul>
      <li key="2014">Connecticut</li>
      <li key="2015">Duke</li>
      <li key="2016">Villanova</li>
    </ul>

    ```

### 최적화

React의 비교 알고리즘이 O(n)으로 최적화되었다지만 수많은 DOM 노드들을 비교하기 위해서는 매우 많은 연산이 필요할 수 있습니다.
이를 최적화하기 위해 React에서는 세 가지 방법을 제시하고 있습니다.

1. ShouldComponentUpdate()
    `ShouldComponentUpdate()` 함수를 통해 컴포넌트 업데이트 여부를 결정할 수 있습니다.
    props와 state의 값에 특정 조건을 만족할 때만 업데이트를 지정하는 방식을 통해 컴포넌트 업데이트를 최적화할 수 있습니다.

2. PureComponent
    `PureComponent` 는 컴포넌트에 `ShouldCompoentUpdate()`  를 기본적으로 수행하는 컴포넌트입니다.
    기존 값과 업데이트되는 값이 동일하다면 업데이트를 수행하지 않습니다.

    ```jsx
    class Student extends React.PureComponent {
        // 나머지 사용법은 기존 컴포넌트와 같습니다
    }
    ```

3. React.memo()

    `React.memo` 는 PureComponent 와 같이 사용할 수 있도록 도와주는 최적화용 HOC(Higher-Order Components)입니다.
    비교방식을 커스텀하여 특정 조건일 때만 리렌더링이 일어나도록 설정할 수도 있습니다.

    ```jsx

    // props.name이 변하지 않는다면 리렌더링 되지 않고 기존 값을 반환
    const NameTag = React.memo(
      (props) => {props.name},
      (prevProps, nextProps) => prevProps.name === nextProps.name
    )

    ```

<참고자료>

1. [MDN Web Docs - DOM 소개](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)
2. [WIT블로그 - [번역]DOM은 정확히 무엇일까?](https://wit.nts-corp.com/2019/02/14/5522)
3. [VELOPERT.LOG - [번역] 리액트에 대해서 그 누구도 제대로 설명하기 어려운 것 – 왜 Virtual DOM 인가?](https://velopert.com/3236)
4. [React Docs - 재조정(Reconciliation)](https://ko.reactjs.org/docs/reconciliation.html)
5. [Husky blog - Reconciliation: React의 렌더링 알고리즘](https://www.huskyhoochu.com/virtual-dom/)
6. [지속가능한 개발 블로그 - React.memo와 useMemo 차이점](https://sustainable-dev.tistory.com/137)

# 2. SSR과 CSR의 비교

## 2-1. SSR(Server Side Rendering)

  서버 사이드 렌더링은 웹 페이지를 호출하기 위해 기존에 자주 사용하던 방법입니다. 서버 사이드 렌더링은 페이지를 이동할 때마다
  서버에 이동할 페이지에 대해 요청을 전송하면 **서버에서 페이지에 대한 렌더링**을 마친 다음 데이터가 결합된 HTML파일을 전송합니다.

  이와 같은 방식은 **매우 짧은 초기 로딩시간**을 가지고 있습니다. 하지만 사용자에게  웹 페이지의 View를 먼저 제공한 후 JS를
  다운로드를 받기 때문에 로딩이 되자마자 인터랙션을 수행할 수는 없습니다. 또한 데이터에 대한 수정이 발생할 때마다 서버에 관련 정보를
  요청하여 응답을 받아와야하기 때문에 수많은 사용자 요청이 발생하게 될 경우 **서버에 심각한 부하**가 가해지는 상황이 발생할 수 있습니다.

## 2-2. CSR(Client Side Rendering)

  클라이언트 사이드 렌더링은 모바일 시대가 도래하면서 성능이 낮은 모바일에 웹 페이지를 최적화하기 위해 등장한 방법입니다.
  클라이언트 사이드 렌더링은 **서버로부터 모든 리소스(HTML, CSS, JS)를 전달받은 다음** 데이터를 결합하여 렌더링한 웹 페이지를
  사용자에게 제공합니다.

  클라이언트 사이드 렌더링 방식은 웹 페이지를 구성하는 모든 리소스를 결합하는 작업을 진행해야하기 때문에 **초기 로딩 속도가 느리지만**
  JS가 결합되어있기 때문에 로딩이 완료되면 바로 사용자 인터랙션을 수행할 수 있어 사용자에게 뛰어난 UX 환경을 제공합니다. 또한 사용자
  요청을 클라이언트 내에서 처리할 수 있어 **서버의 부담을 훨씬 줄이는** 효과를 보일 수 있습니다.

### SPA

  클라이언트 사이드 렌더링 방식을 활용하여 웹 페이지를 구성하기 위해서 SPA(Single Page Application) 라는 개념이
  새로 등장하게 되었습니다. SPA란 **웹 사이트의 전체 페이지를 하나의 페이지에 담아** 동적으로 화면을 바꿔가며 표현하는 개념을
  의미합니다. 이를 통해 라우팅 동작도 서버에 요청을 보내는 것이 아닌 클라이언트 내에서 자체적으로 처리할 수 있어 더 빠른 웹페이지를
  구성할 수 있습니다.

  하지만 SPA는 **JS로 인한 DOM 조작이 빈번하게 발생**하므로 브라우저의 성능이 저하된다는 문제점을 가지고 있습니다.
  SPA 프레임워크인 React, Vue 에서는 이 문제를  Virtual DOM 을 사용하여 DOM 변경을 최소화하는 기법을 사용합니다.
  이를 통해 SPA의 단점을 줄이면서 훨씬 인터렉티브한 웹 사이트를 만들 수 있습니다.

## 3. 장단점 비교

1. 초기 View 로딩 속도

   - SSR은 초기 View를 보여주는 속도가 빠르지만 아직 JS가 결합되지 않은 상태이므로 요청을 바로 수행할 수는 없음
   - CSR은 최초 로딩 시 HTML, CSS , JS 등 모든 리소스를 호출한 뒤 결합하는 과정이 필요하므로
    렌더링 속도가 느리지만 렌더링이 완료되면 요청을 바로 수행할 수 있음

2. SEO(Search Engine Optimization) 문제

   - SSR 방식은 렌더링이 완료된 HTML 파일을 전송하기 때문에 웹 크롤러 봇이 내부 내용을 확인하여 검색최적화 기능을 제공할 수 있음
   - CSR 방식은 내부가 비어있는 HTML 파일을 전송하기 때문에 웹 크롤러 봇을 통한 검색최적화 기능이 정상적으로 이루어지지 않음

3. 보안 문제

   - SSR은 사용자에 대한 정보를 웹 서버에 저장한 후 세션으로 관리할 수 있어 보안성이 높음
   - CSR은 사용자에 대한 정보를 쿠키나 로컬 스토리지에 저장하여 사용하므로 XSS 공격 등에 매우 취약함

<참고 자료>

1. [Naver D2 - 어서 와, SSR은 처음이지? (도입 편)](https://d2.naver.com/helloworld/7804182)
2. [_Ibee blog - 서버사이드 렌더링 그리고 클라이언트 사이드 렌더링](https://asfirstalways.tistory.com/244)
3. [goodGid blog - 서버 사이드 렌더링(SSR)과 클라이언트 사이드 렌더링(CSR)](https://goodgid.github.io/Server-Side-Rendering-and-Client-Side-Rendering/)
4. [Husky 기술블로그 - SPA(Single Page Application) 이란?](https://www.huskyhoochu.com/what-is-spa/)
