---
title: "2021.07.20 HTTP 공부"

categories:
   - study

tags:
   - study
   - HTTP
   - Backend
   - 통신

comments: true
toc: true
toc_sticky: true

date: 2021-07-20 14:48 +0900
last_modified_at: 2021-07-22 15:10 +0900
---

<strong>HTTP</strong>를 공부했다. 학부 3, 4학년때 이미 배운 내용이긴 하지만, 설명할 수 없으면 모르는 내용으로 생각하기로 결정했다. 그런 맥락에서 HTTP는 절반 정도만 아는 내용이었고, 확실히 해두는 것이 나을 것 같아서 새로 정리했다.<br><br>

# 1. HTTP 란?
<strong>HTTP</strong>는 <strong>H</strong>yper<strong>T</strong>ext <strong>T</strong>ransfer <strong>P</strong>rotocol로, 말 그대로 <strong>Hypertext</strong>를 전송하는 프로토콜이다. <strong>서버</strong>와 <strong>클라이언트</strong> 사이에서 어떻게 Hypertext를 교환할 것인지 정해놓은 규칙이다.<br>
<strong>TCP/IP</strong> 프로토콜의 <strong>Application</strong> 계층의 프로토콜이며, 기본 포트 번호는 _80_ 번이다.<br>
<strong>Stateless</strong>한 특성을 지니고 있는데, 각각의 데이터 요청을 독립적으로 관리하는 특성을 가졌다.<br>

## 1. HyperText 란?
> Hypertext는 Hyperlink를 통해 독자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트이다.<br>

링크에 따라 그 차례가 바뀌는 <strong>비선형적</strong>인 텍스트를 의미한다.<br>
책으로 생각하면 저자 서론 -> 목차 -> 1장 1문단 이 아닌 목차 -> 193페이지(?) 와 같이 원하는 곳으로 바로 연결되는 것으로 생각해볼 수 있다.<br><br>
2021년 지금 와서는 당연한 기술이지만, 그때 당시엔 획기적인 기술이었기 때문에 Hyper- 가 붙었다고 한다.<br/><br/>

## 2. URL 이란?
<strong>URL</strong>은 <strong>U</strong>niform <strong>R</strong>esource <strong>L</strong>ocators로, 서버에 <strong>자원</strong>을 요청하기 위한 영문 주소이다.<br>
_protocol://host:port/path?query_ 의 구조로 구성된다.<br><br>
현재 페이지의 URL을 예로 들면, http://bye0nys.github.io/study/http-study/ 에서<br>
- protocol : http
- host:port : bye0nys.github.io
- path : study/http-study

로 볼 수 있다.<br><br>

# 2. HTTP의 구조
HTTP는 크게 두 가지로 구성되어 있는데, 바로 <strong>요청(Request)</strong>과 <strong>응답(Response)</strong>이다.

## 1. HTTP Request
<strong>HTTP Request</strong>는 <strong>브라우저</strong>에서 서버에 특정 동작을 <strong>요청</strong>할 떄 사용된다.<br><br>
<strong>HTTP Request Methods</strong>를 사용하며, <strong>HTTP Verbs</strong>라고 불린다. 다음과 같은 몇 가지 Verbs들이 있다.
- GET : 새로운 자원에 대한 요청
- POST : 새로운 자원을 생성
- PUT : 존재하는 자원 변경
- DELETE : 존재하는 자원 삭제
- HEAD : 서버 헤더 정보
- OPTIONS : 서버의 옵션

## 2. HTTP Response
<strong>HTTP Response</strong>는 <strong>서버</strong>에서 설정해준 <strong>응답</strong> 정보로, 브라우저에게 응답해주는 정보이다.<br><br>
<strong>HTTP Status Code</strong>를 사용하며, 다음과 같은 몇 가지 Code들이 있다.
1. 2xx : 성공
   - 200 : GET 요청 성공
   - 204 : No Content
   - 205 : Reset Content
   - 206 : Partial Content
<br><br>

2. 3xx : Redirection -> 새로운 주소로 리다이렉트
   - 301 : 요청 자원이 새 URL에 존재
   - 303 : 요청 자원이 임시 주소에 존재
<br><br>

3. 4xx : 클라이언트 에러
   - 400 : Bad Request
   - 403 : Forbidden
   - 404 : Not Found
<br><br>

4. 5xx : 서버 에러
   - 501 : 요청 동작을 서버가 수행할 수 없음
   - 503 : 서버의 과부하, 유지 보수 등
<br><br>

# 3. 배운 점
HTTP는 개인정보의 입력이 필요한 사이트에서 정보가 도용될 수 있기 때문에, 현재는 <strong>HTTPS(HyperText Transfer Protocol over Secure Socket Layer)</strong>이 더 많이 사용된다. HTTPS는 HTTP 프로토콜에서 <strong>SSL</strong>, <strong>TLS</strong> 프로토콜을 통해 Session 데이터를 암호화한다.<br><br>
HTTP는 통신 과정에서 많이 사용되는 가장 기본적인 Application 계층 프로토콜인만큼 꼭 알아둘 필요가 있었다. 기본적인 내용들인데도 몰랐던 만큼 기본들을 더 확실히 하고 넘어갈 필요가 있어보인다.