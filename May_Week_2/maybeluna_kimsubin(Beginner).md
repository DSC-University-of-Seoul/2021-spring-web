# 생활코딩과 함께하는 WEB

## WEB02(JavaScript)-210510

### JavaScript란?

웹페이지를 동적으로 만들어주는 **JavaScript**  

- 사용자랑 상호작용
- JavaScript 코드 따라 body 태그 바꿔 줌

### JavaScript와 HTML

html은 정적 JavaSript는 동적이라서 계산기처럼 사용도 가능  

1. JavaScript를 사용을 알릴 때 `<script></script>` 태그를 사용  

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <script>
      document.write(1+1);
    </script>
    1+1  
  </body>
</html>
```

2. on 속성의 속성 값으로 JavaScript 사용

 on 속성 뒤는 무조건 JavaScript가 오는데 이러한 속성을 **event**라고 한다.   
 웹브라우저 위에서 여러 사건 중 유용한 이벤트를 정의해놓았다. 

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
  /** 
  input은 버튼, alert은 경고창
  **/
    <input type="button" value="Oh my God" onclick="alert('Oh my God')">
    <input type="text" onchange="alert('cahnged')">
    <input type="text" onkeydown="alert('key down')">
  </body>
</html>
```

3. 콘솔로 JavaScript 사용

이미 만들어진 웹사이트를 필요에 따라 사용 가능

# 클론코딩 해보기 전 주요 개념

## CSS Position-210511

### value

1. static: 정적
2. realative: 기존 위치 기준 이동
3. absolute: 아이템이 담겨있는 박스 기준 이동
4. fixed: 웹페이지 기준 이동
5. stickiy: 스크롤링 되어도 원래 자리에 배치
6. inherit: 부모 태그 속성갑 상속

### 좌표 지정

- top
- bottom
- left
- right

## CSS flexbox-210512

### 속성값

1. 박스 적용
  - display
  - flex-direction: 중심축 결정
  - flex-wrap: wrapping
  - flex-flow: flex-direction + flex-wrap
  - justify-content: 중심축 정렬
  - align-items: 반대축 정렬 (아이템 전체의 위치)
  - align-content: 반대축 정렬 (라인들의 정렬)

2. 아이템 적용
  - order: 순서
  - flex-grow: 아이템이 커질 때 변화
  - flex-shrink: 아이템이 줄어들 때 변화
  - flex-basis: 아이템이 기본적으로 차지하는 비율
  - align-self: 아이템 하나 당 정렬

### 축

중심축과 반대축이 존재  

수직축을 중심축을 지정하면 수평축이 반대축으로 지정  

수평축을 중심축으로 지정하면 수직축이 반대축으로 지정  

# 클론코딩01-210513~14

[nev](https://maybeluna.github.io/project01_clonecoding/nav/nav_index.html)

느낀점😊  
> css 파트까지 끝내고 실제 프로젝트를 해보려고 했다.  
> 유튜브에 정적인 웹사이트 추천해준 것을 예시를 보고 만들었는데  
> 처음에 어디서부터 시작해야할 지 모르겠어서 막막했다.  
> 그러나 박스를 나누고 하나하나 박스를 채우면서 완성했다.
> 박스를 사용할 때마다 박스의 용도가 맞나? 고민을 하게 되었는데  
> 그 부분은 차차 익혀나가야겠다.
