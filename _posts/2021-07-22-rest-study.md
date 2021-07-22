---
title: "2021.07.22 REST API 공부"

categories:
   - study

tags:
   - study
   - HTTP
   - Backend
   - 통신
   - REST
   - API

comments: true
toc: true
toc_sticky: true

date: 2021-07-22 16:37 +0900
last_modified_at: 2021-07-22 16:37 +0900
---

오늘은 <strong>API</strong>와 <strong>REST</strong>, <strong>REST(RESTful) API</strong>를 공부했다. HTTP에 이어 기초 내용들을 복습하고자 API부터 시작했다. API를 사용하고 개발하는 것에 있어 <strong>RESTful</strong>은 꽤 중요한 개념이라고 생각됐다. 따라서 REST API 이전에 API에 대한 내용을 먼저 정리하면서 시작한다.<br><br>

# 1. API 란?
<strong>API</strong>란 <strong>A</strong>pplication <strong>P</strong>rogramming <strong>I</strong>nterface로, <strong>응용 프로그램</strong>에서 사용할 수 있도록, _OS_ 나 _프로그래밍 언어_ 가 제공하는 기능을 제어할 수 있게 만든 <strong>'인터페이스'</strong>이다. 가장 흔히 볼 수 있는 _Java.io_, _Java.util_등은 _Java_ 에서 제공하는 <strong>API</strong>이고, _Android SDK(Software Development Kit)_ 도 <strong>API</strong>에 해당한다. 이전에 개인 암호화폐 퀀트를 설계하며 _Upbit API_ 를 배웠던 것도 기억에 남는다.<br><br>
이처럼 어떠한 방식으로 정보를 요청해야 하고, 어떤 데이터를 제공받을 수 있는지 등이 담겨있는 규격이고, _프로토콜_ 과 비슷한 느낌으로 이해했다.<br><br>
API는 다음과 같은 두 가지 특징이 있다.

1. DB 접근성 부여 : <strong>허용된 사람</strong>에게만 <strong>DB 접근성</strong>을 부여해준다.
2. 접속 표준화 : OS, 기계에 상관없이 <strong>접속을 표준화</strong>해 <strong>범용</strong>으로 사용할 수 있다.

<br>
이러한 기능을 위해 개발자는 어떤 <strong>표준</strong>이나 <strong>가이드라인</strong> 등을 준수해야 하고, 각각 <strong>SOAP</strong>와 <strong>REST</strong>로 나눌 수 있다. 이 페이지에서는 <strong>REST</strong>를 중점으로 다룰 예정이므로, SOAP는 간단히 설명하고 넘어갈 것이다.<br>

## 1. SOAP API
<strong>SOAP API</strong>에 대한 설명은 [Red Hat](https://www.redhat.com/ko/topics/integration/whats-the-difference-between-soap-rest)을 참고했다.<br>
<strong>SOAP</strong>는 <strong>S</strong>imple <strong>O</strong>bject <strong>A</strong>ccess <strong>P</strong>rotocol로, 이름에서 알 수 있듯이 <strong>'프로토콜'</strong>이다. 프로토콜이므로 REST보다 <strong>복잡성</strong>과 <strong>오버헤드</strong>가 크고, <strong>보안</strong>을 준수해야 하는 상황에서 적합하다. 따라서 주로 _은행용 모바일 앱_ 등 보안 수준이 높아야 하는 상황에서 쓰이는 것으로 알려져 있다. 

## 2. SOAP vs REST
아직 REST에 대해선 서술하지 않았지만, <strong>SOAP</strong>와 <strong>REST</strong>를 간략하게 비교해 보면 다음과 같다.<br>

|차이점|SOAP|REST|
|:-----:|:-----:|:-----:|
|기능|기능 위주 : 구조화된 정보 전송|데이터 위주 : 데이터를 위해 리소스에 접근|
|데이터 포멧|XML|텍스트, HTML, XML, JSON 등|
|보안|WS-Security, SSL|SSl, HTTPS|
|대역폭|상대적으로 더 많은 리소스와 대역폭 필요|상대적으로 적은 리소스, 가벼운 무게|

<br>
위의 표에 따르면 REST는 SOAP보다 더욱 _유연한 구현_ 이 가능하므로, _IoT_ 와 _모바일 애플리케이션_ 등의 개발에 효과적이다.<br><br>

# 2. REST 란?
바로 위에서 매우 짧게 본 <strong>REST</strong>에 대해 알아보자.<br>
<strong>REST</strong>는 <strong>RE</strong>presentational <strong>S</strong>tate <strong>T</strong>ransfer의 약자로, <strong>자원에 대한 주소</strong>를 지정하여 해당 자원의 정보를 전송하는 것이다. 정보의 전달과정에서는 _XML_, _JSON_ 등을 통해 데이터를 주고받는다.<br><br>
<strong>HTTP URI(Uniform Resource Identifier)</strong>을 통해 자원을 명시하고, <strong>HTTP Method(POST, GET, PUT, DELETE)</strong>를 통해 자원에 대한 <strong>CRUD Operation(Create, Read, Update, Delete)</strong>을 적용한다.

## 1. REST의 장점
REST를 활용하면 다음과 같은 장점이 있다.
1. <strong>HTTP 프로토콜의 인프라(Method 등)</strong>을 그대로 사용하므로, 별도의 인프라 구축 필요가 없다.
2. 메시지가 <strong>의도하는 바가 명확</strong>하다.
3. <strong>HTTP 프로토콜을 따르는 모든 플랫폼에서 사용 가능</strong>하다.
4. <strong>멀티 플랫폼 통신</strong>이 가능하다.

## 2. REST의 구성 요소
REST를 이루는 구성 요소는 다음과 같다.

### 1. 자원(URI)
모든 자원엔 _ID_ 가 존재하고, 자원들은 Server에 존재한다.<br>
자원을 구별하는 _ID_ 는 _/groups/:group_id_ 와 같은 <strong>HTTP URI</strong>이다.

### 2. 행위(HTTP Method)
REST는 <strong>HTTP Method</strong>를 활용하며, HTTP Method는 _GET_, _POST_, _PUT_, _DELETE_ 등의 Method를 제공한다.

### 3. 표현
표현은 <strong>Client의 Request</strong>에 대한 <strong>Server의 Response</strong>이며, _Text_, _JSON_, _XML_, _RSS_ 등 여러 형태의 표현이 있다. 위에서 언급한 바와 같이, JSON과 XML을 가장 많이 사용한다.<br><br>

## 3. REST의 특징
REST는 다음과 같은 특징이 있고, <strong>HTTP의 여러 특징들</strong>이 그대로 적용된다.

### 1. Server-Client 구조
Server-Client 구조이며, <strong>자원이 있는 쪽이 _Server_</strong>, <strong>자원을 요청하는 쪽이 _Client_</strong>이다.

### 2. Stateless (무상태)
HTTP가 <strong>Stateless</strong> Protocol이므로, 이를 활용하는 REST도 <strong>Stateless</strong>의 특징을 갖는다.

### 3. Cachable
HTTP의 가장 큰 특징인, <strong>캐싱 기능</strong>을 그대로 적용할 수 있다.

### 4. Layered System
_Client_ 는 <strong>REST API Server</strong>만 호출하며, <strong>REST Server</strong>는 <strong>다중 계층</strong>으로 구성될 수 있다.<br>
API Server는 순수 비즈니스 로직을 수행하며, 그 앞단에 보안, 암호화, 사용자 인증 등 추가 기능을 구현할 수 있다. 또한, 프록시와 게이트웨이 등 중간 매체를 사용할 수도 있다.

### 5. Uniform Interface
URI로 지정한 Resource에 대한 조작을 <strong>통일되고 한정적인 인터페이스</strong>로 구현하며, HTTP를 따르는 몯느 플랫폼에서 사용 가능하다.<br><br>

# 3. REST API
위에서 REST에 대해 알아봤는데, <strong>REST API</strong>는 <strong>REST</strong> 기반으로 <strong>서비스 API를 구현한 것</strong>을 의미한다.<br>
이러한 설계엔 몇가지 기본 규칙이 있고, 가장 기초적인 규칙들은 다음과 같다.

## 1. REST API 설계의 기본 규칙

1. URI는 정보의 자원을 표시한다.
* 동사보다는 명사, 대문자보다는 소문자를 사용한다.
* <strong>Document</strong> 이름으로는 단수 명사를 사용한다.
* <strong>Collection</strong> 이름으로는 복수 명사를 사용한다.
* <strong>Store</strong> 이름으로는 복수 명사를 사용한다.<br><br>

2. 자원에 대한 행위는 HTTP Method로 표현해야 한다.
* URI에 HTTP Method가 들어가면 안된다.
* 경로 부분에서 수시로 변하는 부분은 유일한 값으로 대체해야 한다.<br><br>

이 외에도, 다음과 같은 몇 가지 규칙이 더 있다.
* /는 계층 관계를 나타내는 데 사용한다.
* '-'는 URI 가독성을 높이는 데 사용한다.
* '_'는 URI에 사용하지 않는다.
* URI 경로에는 소문자를 사용한다.
* 파일의 확장자는 URI에 포함하지 않는다.<br><br>

잘 설계된 예와 좋지 못한 예를 찾고 싶었지만, 쉽지 않았다(...)<br><br>
그래도 잘 설계된 예로는 Microsoft의 문서를 예로 들고 싶다.<br>
"https://docs.microsoft.com/ko-kr/azure/architecture/best-practices/api-design"<br>
적절한 '-'의 사용과 중복되지 않는 이름 덕분에 보기 깔끔하고 의도하는 바를 파악하기 쉽다.<br><br>
좋지 못한 예는 굳이 찾기는 힘들지만, <strong>Query</strong>의 조건을 주지 않고 _find_, _get_ 등이 URL에 섞여있는 웹, 혹은 사이트의 URL이 하나로 동일한 웹 등을 예로 들고 싶다.<br><br>
사실 적으면서 느낀 것이지만 처음부터 규칙에 맞게 잘 작성해놓는 습관을 만들어놓으면 나중에 편하지만, 이미 어느정도 구축된 상태라면 일일히 수정하는게 참 힘들 것 같다..<br><br>

# 4. RESTful API
이름에서 알 수 있듯이, <strong>REST를 잘 준수하는 API</strong>를 <strong>RESTful API</strong>라고 한다. REST API를 제공하는 웹 서비스를 <strong>RESTful</strong> 하다고 할 수 있다. 별다른 공식이나 표준은 없지만, REST의 원리를 잘 따르면 RESTful로 부른다고 한다.<br><br>
이는 이해하기 쉬운 REST API를 만들기 위한 목적도 있고, 범용성과 호환성을 높이기 위한 목적도 있다. 따라서 웹 개발을 시작하기 위해선 이러한 REST 규칙들에 익숙해질 필요가 있을 것이다.<br><br>

# 5. 배운점
사실 직접 해보지 않는 이상 감이 잘 오지 않을 것 같다. 머릿속으로는 저런 규칙들을 잘 지켜야지 생각하지만 막상 내가 무엇인가를 개발하는 입장에서는 신경쓰기 까다로울 것 같은 느낌..?<br><br>
어쨋든 어떠한 규칙이나 표준에 익숙해져야 다른 REST API들을 다룰 때 빠르게 접근할 수 있을 것 같으니, REST 규칙들에 빠르게 익숙해져야겠다.