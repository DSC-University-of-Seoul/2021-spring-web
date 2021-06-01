# 클론코딩과 함께하는 WEB

## 클론코딩03-210524~26

[bmax](https://maybeluna.github.io/project01_clonecoding/bmax/bmax_index.html)

### 클론코딩03 목표

1. 데스크탑 웹사이트 완성하기
2. 모바일 버전 웹사이트 완성하기
3. 웹호스팅을 통해 나타나는 이미지 에러 문제 해결하기

### 클론코딩03 과정

1. pc ver 웹페이지를 구성
2. 모바일 버전으로 만들기 위해 모바일 사이즈에 맞추어 미디어 쿼리 적용
3. 미디어 쿼리 적용 후 기존에 존재하는 것 재정렬

### 클론코딩03 문제점

- 모바일 버전 때 빈 공간이 채워지지 않는 문제
- 모바일 버전 때 텍스트 정렬이 이상하게 나타나는 문제
- 모바일 버전 때 메뉴 버튼이 나타나지 않는 문제

### 클론코딩03 해결책

```css
/* 1번 padding 재적용 */

@media screen and (max-width: 750px) {
    nev{
        padding: 20px 50px;
    }
    .ad{
        padding: 100px 20px;
        display: flex;
        flex-direction: column;
    }
    .medi_info .detail_info{
        display: flex; 
        font-size: 20px;
    }
}
/* 2번 텍스트 정렬 */
@media screen and (max-width: 750px) {
.medi_info .detail_info{
        display: flex; 
        font-size: 20px;
    }

    .threeb{
        display: flex;
        flex-direction: column;
        padding: 0;
    }

    .threeb ul{
        display: flex;
        flex-direction: row;
    } 

    .top{
        flex: 1;
    }

    .top span{
        font-size: 36px;
        max-width: 120px;
    }

    .mid{
        font-size: 14px;
        flex: 1;
    }
    
    .bottom{
        font-size: 3px;
        flex: 1;
        text-align: start;
    }
}
/* 3번  메뉴 버튼을 position을 이용해 고정 */
@media screen and (max-width: 750px) {    
    nev button{
        display:inline ; 
        background-color: var(--lightblue_color);
        border: none;
        position: relative;
        top: 100%;
        left: 100%;
    }
}
```

느낀점😊  
> 자바스크립트까지 하려고 했는데 아직 문법에 익숙치 않아 미디어쿼리만 적용했다.
> 하나하나 적용하니까 노가다 같은 느낌이 든다.
> 이러면서 프레임워크에 대한 중요성을 깨닫는 건가?
> 다음에는 전반적인 틀만 빌려와서 내 프로젝트를 만들어보고 싶다.

## Markdown.lint ERROR-210525

마크다운 형식이 맞는 지 확인하기 위해서
실행시킨 Markdown.lint
그러나 자꾸 `markdwonlint command not found`가 뜨면서 Error

### 해결책과 결과

1. 웹 비기너 팀에서 알려준 window 환경 변수 설정  
-> 환경변수가 이미 설정되어 있음에도 실패  
2. CMD 창에서 실행  
-> 여전히 똑같은 에러 발생  
3. NPM 사이트에서 추천해준 설치 방법으로
`npm install -g markdownlint-cli` 실행  
-> ![캡처](https://user-images.githubusercontent.com/72259053/120367284-a4d02500-c34b-11eb-8ce7-dba56409c564.png)
위와 같이 뜨면서 전혀 넘어가지 않음  

### 결론

모든 방법을 실패해서 일단 길이 제외하고
VScode 내에 있는 markdown.lint를 사용할 예정
아마도 윈도우 문제로 보인다고 하심  

느낀점😊  
> 처음 시작할 때 git이 엉망진창 꼬였을 때가 생각 났다..!
> 노트북을 바꾸고 싶다는 생각이 점점 더 강해진다!  

## 클론코딩04 - 210527~28

### 클론코딩04 목표

- 내가 좋아하는 미디어 소개하는 웹사이트 만들기
- 분야는 영화, 책, 음악이며 각 분야마다 다른 효과(Javascript) 적용

### 클론코딩04 과정

1. 자료 수집
2. HTML, CSS 활용해서 기초 웹페이지 뼈대 적용
3. Javascript 추가 적용

### 클론코딩04 문제점

캐러셀과 같은 Javascript를 적용하려고 남들 것을 따라하는데,
생활코딩 기초 강의만 듣고 적용하기에는 이해가 되지 않음
그래서 Jquery를 사용할까 고민 중  

### 클론코딩04 해결책

일단 기초 바닐라 Javascript를 좀 더 연습해보고
다시 한번 적용해서 어떻게 구현되는 지 확인해볼 생각이다.  

느낀점😊  
> 이론을 아는 것보다 빠르게 적용해서 익히는게 먼저구나라는 생각이 든다.
> 더 손에 익히는 방향으로 공부해야지!  
