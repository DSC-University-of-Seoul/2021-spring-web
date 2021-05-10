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
