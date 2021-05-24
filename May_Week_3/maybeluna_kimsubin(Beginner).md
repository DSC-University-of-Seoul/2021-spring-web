# 생활코딩과 함게하는 WEB

## WEB02(Javascript)-210517

### Data type

숫자(Number)과 문자열(String)  

숫자: 수로만 포현되어 있는 것 ex. 1, 23  
문자열: 따옴포료 감싸져 있는 데이터 타입 ex. '11', "hello"  

위 데이터들은 propoerty와 method를 통해 다양한 값을 도출할 수 있다.

```javascript
# 숫자
>1+1
2

# 문자열
>'1'+'1'
11

# property & method
>'hello'.length
5
>'hello'.toUpperCase()
"HELLO"
>'hello'.indexOf('hel')
0
'               hello         '.trim()
>"hello"
```

### 변수와 대입 연산자

변수: 변할 수 있는 값 ex. var name = "subin"; 
상수: 변하지 않는 것  ex. 2, 4

변수를 사용할 경우 변화하는 환경 속에서 적용하기 쉽다.

### 제어할 태그 선택

1. 제어할 선택자 추가 `document.querySelector('body')`
2. 제어할 선택자의 태그 추가 `.style`
3. 제어할 태그의 속성값 부여 `.backgroundColor = 'black';`

```html
    <input type="button" value="night" onclick="
    document.querySelector('body').style.backgroundColor = 'black';
    document.querySelector('body').style.color='white';
    ">
    <input type="button" value="day" onclick="
    document.querySelector('body').style.backgroundColor = 'white';
    document.querySelector('body').style.color='black';
    ">
```

### 프로그램, 프로그래밍, 프로그래머

프로그램: 시간의 관계 없이 실행되도록 하는 방법 ex. html  
프로그래밍: 시간의 순서에 따라 실행되도록 하는 방법 ex. Javascript  


### Boolean

Boolean: True와 False 단 두개의 데이터로 이루어진 데이터 타입

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <h1>Comparison operators & Boolean</h1>
    <h2>===</h2>
    <h3>1===1</h3>
      <script>
        document.write(1===1);
      </script>

    <h3>1===2</h3>
      <script>
        document.write(1===2);
      </script>

    <h3>1&lt;2</h3>
      <script>
        document.write(1<2);
      </script>

    <h3>1&lt;1</h3>
      <script>
        document.write(1<1);
      </script>
  </body>
</html>
```

### 조건문

조건에 따라 하는 일이 분기되어 실행

`if(조건){실행문} else{실행문}` 형식으로 실행됨

```html
    <input id="night_day" type="button" value="night" onclick="
    if(document.querySelector('#night_day').value === 'night'){
    document.querySelector('body').style.backgroundColor = 'black';
    document.querySelector('body').style.color='white';
    document.querySelector('#night_day').value = 'day';
    } else {
    document.querySelector('body').style.backgroundColor = 'white';
    document.querySelector('body').style.color='black';
    document.querySelector('#night_day').value = 'night';
    }"
    >
```

### 리팩토링

리팩토링이란?  
동작하는 것은 그대로 나두고 코드 자체를 효율적으로 만드는 것

- 코드의 가독성을 높이고 유지보수를 하기 편리하게 만듦
- 중복된 코드를 낮추는 방향으로 코드를 다시 개선하는 작업

중복을 **제거**하는 것이 가장 우선적이다.  

this - 코드가 속한 태그 가리킴
var target - 타겟 지정

```html
    <input type="button" value="night" onclick="
    var target = document.querySelector('body');
    if(this.value === 'night'){
    target.style.backgroundColor = 'black';
    target.style.color='white';
    this.value = 'day';
    } else {
    target.style.backgroundColor = 'white';
    target.style.color='black';
    this.value = 'night';
    }
    ">
```

### 반복문

#### 배열

데이터가 많아짐에 따라 서로 연관된 데이터 정리하는 것  

#### 반복문

반복되는 일을 하게 해주는 것으로  
자바스크립트에서 지원하는 반복문은 **while, for** 등이 있음

```html
      <script>
        document.write('<li>1</li>');
        var i = 0;
        while(i < 3){
          document.write('<li>2</li>');
          document.write('<li>3</li>');
          i = i + 1;
        }
        document.write('<li>4</li>');
      </script>
```

```html
    <script>
      var cowokers = ['hani', 'yohan', 'hudson', 'olly', 'gabi'];
    </script>
    <ul>
      <script>
        var i = 0;
        while(i < cowokers.length){
        document.write('<li>'+cowokers[i]+'</li>');
        i = i+1;
      }
      var k = 0;
      while(i < cowokers.length){
      document.write('<li><a href="http://a.com'+cowokers[i]+'"></a>'+cowokers[i]+'</li>');
      i = i+1;
    }
      </script>
```

#### 배열과 반복문 활용

```html
<!doctype html>
<html>
  <head>
    <title>Your best Tech device</title>
    <meta charset="utf-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <h1><a href="index.html">Tech Device</a></h1>
    <input type="button" value="night" onclick="
    var target = document.querySelector('body');
    if(this.value === 'night'){
    target.style.backgroundColor = 'black';
    target.style.color='white';
    this.value = 'day';

      var alist = document.querySelectorAll('a');
      var i = 0;
      while(i < alist.length){
        alist[i].style.color = 'powderblue';
        i = i + 1;
      }

    } else {
    target.style.backgroundColor = 'white';
    target.style.color='black';
    this.value = 'night';

    var alist = document.querySelectorAll('a');
    var i = 0;
    while(i < alist.length){
      alist[i].style.color = 'blue';
      i = i + 1;
    }
    }
    ">
    <div id ="grid">
      <ul>
        <li><a href="1.html">Laptop</a></li>
        <li><a href="2.html">Tablet pc</a></li>
        <li><a href="3.html">Smart phone</a></li>
      </ul>
      <div id = "article">
        <h2>Lenovo ThinkPad</h2>
        <a href="https://www.lenovo.com/us/en/thinkpad"><img src="Lenovo.jpg" width="50%"></a>
        <p>The way we compute. The way we do business.
          The way we live our lives. 
          Every time business has gone somewhere new, 
          discovered something different, 
          or done something revolutionary, 
          chances are, ThinkPad was there.
        </p>
        <input type="button" value="night" onclick="
      </div>
    </div>
  </body>
</html>
```

## WEB02(Javascript)-210518

### 함수

함수란?  
코드가 많아지면 그 코드를 정리하기 위한 도구  

```html
    <script>
      function nightDayHandler(self){
        var target = document.querySelector('body');
        if(self.value === 'night'){
        target.style.backgroundColor = 'black';
        target.style.color='white';
        self.value = 'day';

          var alist = document.querySelectorAll('a');
          var i = 0;
          while(i < alist.length){
            alist[i].style.color = 'powderblue';
            i = i + 1;
          }
        } else {
        target.style.backgroundColor = 'white';
        target.style.color='black';
        self.value = 'night';

        var alist = document.querySelectorAll('a');
        var i = 0;
        while(i < alist.length){
          alist[i].style.color = 'blue';
          i = i + 1;
        }
        }
      }
    </script>
```

`function 함수명(){}` 형태로 사용하며 이를 다양하게 활용할 수 있다.

### 객체

객체란?  
함수 기반 위에 존재하는 것으로  
연관된 함수와 변수를 grouping하여 정리정돈하기 위한 도구  

객체에 있는 함수는 Method라고 한다.  

배열이 순서대로 저장한다면,  
객체란 순서는 상관없지만   
나중에 불러오기 위해서 이름과 함께 저장한다.  

#### 객체 만들고 불러오기

```html
        <script>
        /*객체 만들기 */
          var roommates = {
            "student":"dayoung",
            "woker":"yeonju"
          };
          /*객체 불러오기*/
          document.write("studnet:"+roommates.student+"<br>");
          document.write("woker:"+roommates.woker+"<br>");
          /*객체 추가하기*/
          roommates.scientist = "jaeun";
          roommates["art designer"] = "serin";
          document.write("art designer:"+roommates["art designer"]+"<br>");
        </script>
```

#### 객체와 반복문

배열은 index 객체는 key  

```html
      <script>
        for(var key in roommates) {
        document.write(key+':'+roommates[key]+'<br>');
        }
      </script>
```

#### Property & Method

객체에 소속된 함수 Method  
객체에 소속된 변수 Property  

```html
      <script>
        roommates.showAll = function(){
          for(var key in this) {
          document.write(key+':'+this[key]+'<br>');
          }
        }
       roommates.showAll();
      </script>
```

함수 소속된 객체를 가리키는 기호가 `this`로 중복 제거 가능  

#### 객체 활용

```html
    <script>
    var Links = {
      setColor:function (color){
        var alist = document.querySelectorAll('a');
        var i = 0;
        while(i < alist.length){
          alist[i].style.color = color;
          i = i + 1;
        }
      }
    }
    var Body = {
      setColor:function (color){
        document.querySelector('body').style.color = color;
      },
      setBackgroundColor:function (color){
        document.querySelector('body').style.backgroundColor = color;
      }
    }
        function nightDayHandler(self){
          var target = document.querySelector('body');
          if(self.value === 'night'){
            Body.setBackgroundColor('black');
            Body.setColor('white');
            self.value = 'day';
            Links.setColor('powderblue');
            }
          else {
          Body.setBackgroundColor('white');
          Body.setColor('black');
          self.value = 'night';
          Links.setColor('blue');
          }
        }
    </script> 
```

### 파일로 쪼개기

Javascript 코드만 `.js` 파일로 분리해서 관리

- 새로운 코드를 만들 때 Javascript 파일에서 유지 보수
- 가독성이 좋아지고 코드가 더 명확해짐
- css와 같이 캐시 덕분에 효율적

### library & framework

라이브러리와 프레임워크는 모두 협력을 위해 사용

라이브러리: 부품을 가져오는 것처럼 잘 정리정돈되어 재사용하기 편함  
ex. jQuery  
프레임워크: 공통적인 것은 프레임워크가 만들고 기능에 따라 변경시키는 것  

직접 라이브러리를 다운로드하고 비용 지불하는 것 대신  
CDN을 이용하여 
CDN을 제공하는 기업의 서버에 파일 보관하고,  
`<Script src="">`로 라이브러리 이용  

#### CDN으로 jQuery 사용해보기

1. jQuery를 script로 붙이고
2. Javascript에 `$('a').css('color', color);`로 적용

반복문을 jQuery가 처리  
`$('a')`는 이 웹페이지에 있는 모든 a 태그를 jQuery로 제어  

`ctrl + /` 코드 주석처리 방법  

```html
<script>
var Links = {
  setColor:function (color){
    // var alist = document.querySelectorAll('a');
    // var i = 0;
    // while(i <script alist.length){
    //   alist[i].style.color = color;
    //   i = i + 1;
    // }
    $('a').css('color', color);
  }
}
var Body = {
  setColor:function (color){
    // document.querySelector('body').style.color = color;
    $('body').css('color', color);
  },
  setBackgroundColor:function (color){
    // document.querySelector('body').style.backgroundColor = color;
    $('body').css('backgroundColor', color);
  }
}
    function nightDayHandler(self){
      var target = document.querySelector('body');
      if(self.value === 'night'){
        Body.setBackgroundColor('black');
        Body.setColor('white');
        self.value = 'day';
        Links.setColor('powderblue');
        }
      else {
      Body.setBackgroundColor('white');
      Body.setColor('black');
      self.value = 'night';
      Links.setColor('blue');
      }
    }
</script>    
```

### UI & API

#### UI(User Interface)

사용자가 시스템을 제어하기 위해서 사용하는 조작 장치  

#### API(Application Programming Interface)

애플리케이션을 만들기 위해서 프로그래밍을 할 때 사용하는 조작 장치  
자바스크립트와 API를 결합해서 새로운 응용프로그램 만들 수 있음  

느낀점😊  
> 클론코딩을 해보면서 동적인 환경을 구현해보고 싶었다.  
> 그런데 초반에는 이해가 가다가 객체부분부터 조금 이해가 어려웠다.  
> 그리고 어떠한 활용법이 있는지 아직은 잘 모르겠어서  
> 실제 클론코딩에 적용해보면서 이해해보아야겠다.  

# 클론코딩02-210519~20

[youtube_mobile](https://maybeluna.github.io/project01_clonecoding/nav/nav_index.html)

## process

박스를 나누고 css 파일을 분리하여 디자인을 가미했다.  
가장 중점적으로 본 것은 박스에 맞게 css를 부여하는 것이었다.

## problem

하나가 변하면 동시에 변하기 때문에  
박스 크기 조절 자체가 어려웠다.  
균형있게 만들기 위해서 기존 웹페이지를 좀 더 면밀하게 따져봐야겠다.

느낀점😊  
> 간단한 정적인 웹피이지를 다시 한번 클론 코딩을 해보았다.  
> 지난 첫 코딩보다는 박스를 수월하게 나누었다.  
> 그런데 박스가 많아지니까 감당하기 어려웠다.  
> 이 부분을 어떻게 해결해야할 지 좀 더 고민해보아야겠다.  

# 클론코딩03-210521~210522

[bmax](https://maybeluna.github.io/project01_clonecoding/bmax/bmax_index.html)
