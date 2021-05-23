# Study with Me :)

## REST API는 무엇인가?

REpresentational State Transfer(REST)는 하이퍼미디어를 위한 *소프트웨어 아키텍쳐*입니다.

> 소프트웨어 아키텍처는 소프트웨어 구성요소들 사이에서 유기적인 관계를 표현하는 지침이자 원칙이라 할 수 있습니다.

Application Programming Interface(API)는 어플리케이션 소프트웨어를 구축하기 위한 정의들의 집합으로 표현합니다.

REST API는 웹 상에 존재하는 리소스를 전송 계층에 종속되지 않게 전송하기 위한 인터페이스를 뜻하지만, 
엄격한 의미로는 자원을 정의하고 주소를 작성하는 방법 전반을 일컫기도 합니다.

REST API는 리소스, 메소드, 메세지로 구성됩니다.

* 리소스: 웹사이트에서 특정 자원의 위치를 나타내는 주소(Uniform Resource Identifier)
* 메소드: API가 수행하고자 하는 행위(http method)
* 메세지: 어떤 내용에 대해 처리를 원하는지(http request data, payload)

아래 코드에서 리소스, 메소드, 메세지를 찾아봅시다.

```java
HTTP DELETE, http://suinweb/posts
{
    "posts":{
        "id":"1"
    }
}
```

* suinweb/posts(리소스): suinweb에서 posts라는 자원의 위치를 나타내는 주소
* HTTP DELETE(메소드): API가 수행하고자 하는 삭제 행위
* posts 중 id가 1인 것(메세지): 1을 id로 가지는 posts

종합하자면 위 코드는 suinweb에 있는 posts 중 id가 1인 것을 삭제하라는 의미를 가진 api인 것입니다.

## REST API는 왜 필요한가?

기업들의 취업 공고를 보면 REST API 설계 경험이 있는 사람을 우대한다는 문구를 어렵지 않게 확인할 수 있습니다.
REST API를 어떻게 읽는지는 알겠는데, 이게 어떤 장점을 가지길래 기업의 우대조건에 포함될까요?
놀랍게도 REST의 장점은 쉽게 읽을 수 있다는 점 그 자체입니다.
우리는 API를 구성하는 리소스, 메소드, 메세지만 있다면 그 인터페이스가 의도한 바를 쉽게 파악할 수 있습니다.
이러한 성질은 유지보수가 필수적인 개발 작업에서 큰 장점이라 할 수 있습니다.

또한 REST는 HTTP 프로토콜을 그대로 사용합니다.
REST API를 위한 별도 인프라를 구축할 필요가 없어 간편하게 사용할 수 있고 HTTP 프로토콜을 따르는 모든 플랫폼에서 사용할 수 있습니다.

REST는 stateless한 특징을 가지므로 서버와 클라이언트의 역할이 명확히 분리된다는 장점도 있습니다.
역할 분리는 개발자들의 업무량을 감소시키고 소프트웨어가 플랫폼에 독립적으로 작동하도록 돕습니다.
기본적으로 HTTP 프로토콜 서비스이기만 하다면 다양한 플랫폼에서 서비스를 쉽고 빠르게 개발할 수 있습니다.

확장성이 넓다는 특성은 부작용을 가져오기도 합니다.
어찌보면 마크다운의 특성과도 비슷한데요, REST는 표준화된 공식적인 API 디자인 가이드가 없어 관리가 어렵습니다.
개괄적인 규칙 내에서 개발자들끼리 세부적인 부분을 약속한 후 개발을 해야 하고 한 조직에서의 약속이 다른 조직에서 통하지 않을 가능성도 있습니다.

또다른 REST API의 단점은 http method가 GET, POST, PATCH, POST, PUT, DELETE 등으로 제한적이라는 것입니다.
예를 들어 로그아웃 기능을 구현한다고 했을 때,
user를 DELETE하도록 api를 설계하면 회원탈퇴 기능은 어떤 방식으로 구현할지 고민하게 됩니다.
한정된 http method가 여러 의미를 내포하며 실세계 데이터를 모델링하는 것에 한계를 느낄 수 있습니다.

## 매운맛 - RESTful API 설계 원칙

REST를 제안한 Roy Thomas Fielding의 박사학위 논문 Architectural Styles and
the Design of Network-based Software Architectures에 따르면 REST는 다음 조건을 만족해야 합니다.

1. Starting with the Null Style: 아무것도 없는, www 노드 하나만으로 api 설계를 시작해야 한다.
2. Client-Server: 아키텍처를 단순화하고 작은 단위로 분리함으로써 각 부분이 독립적으로 개선되어야 한다.
3. Stateless: 각 request가 발생된 맥락(state)은 서버에 저장되지 않아야 한다.
4. Cache: 각 자원에 대해 캐시 가능 여부를 지정해야 하고,
캐시 가능한 response는 너무 오래된 자원이 아닌 이상 네트워크 효율을 높인다.
5. Uniform Interface: 일관적인 인터페이스를 사용해 전체 시스템 아키텍처 가시성을 놏여야 한다.
6. Layered System: 시스템을 계층적으로 구성해 각 layer가 독립적으로 동작하도록 해야 한다.
layered system의 단점인 오버헤드는 shared caching를 사용해 중재함으로써 성능을 향상시킨다.ㄴ
7. Code-On-Demand: REST를 사용해 java applet 등 사전 구현된 코드를 적극 사용함으로써 클라이언트를 단순화한다.

## 순한맛 - RESTful API 설계 원칙

매운맛 설계 원칙 외에 저와 같은 [초심자가 쉽게 받아들일 수 있는 원칙](https://url.kr/fxb5tp)도 있습니다!

원칙이 없는 아키텍처에 원칙이라기엔 민망하지만 개발자들의 느슨한 약속으로 이해해도 될 것 같습니다.

 1. URI 작성시 리소스명을 동사가 아닌 명사로 작성하기
 2. 자원 상태를 변경할 때 GET과 query parameter가 아닌 PUT, POST, DELETE 사용하기
 3. URI 작성시 리소스명을 단수가 아닌 복수로 작성하기
 4. 리소스 관계는 계층적으로 구성하기
 5. 직렬화 포멧은 http 헤더 사용하기
 6. API에 더 나은 접근성을 제공해주는 HATEOAS 사용하기
 7. 요청에 대한 response를 반환하는 리스트에 필터링, 정렬, 페이징 기능을 제공하기
 8. API 버전을 관리하고 소수점 표기 피하기
 9. HTTP 상태 코드를 이용해 API 에러 핸들링하기
 10. REST API가 일부 프록시 서버에만 제한되지 않도록 하기 위해 HTTP method를 오버라이드하기

## References

* <https://bcho.tistory.com/953?category=252770>
* <https://bcho.tistory.com/954?category=252770>
* <https://ko.wikipedia.org/wiki/REST>
* <https://meetup.toast.com/posts/92>
* <https://wallees.wordpress.com/2018/04/19/>rest-api-%EC%9E%A5%EB%8B%A8%EC%A0%90/>
* <http://www.incodom.kr/REST>
* <https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.html>
