---
title: "2021.07.15 HTML 공부"

categories:
   - study

tags:
   - study
   - HTML
   - Frontend

comments: true
toc: true
toc_sticky: true

date: 2021-07-15 18:52 +0900
last_modified_at: 2021-07-15 18:52 +0900
---

웹개발을 공부하기 위해 먼저 <Strong>HTML</Strong>을 공부했다. 현재 사용중인 _jekyll_ 과 그 테마인 _minimal-mistakes_ 도 거의 다 <Strong>HTML</Strong>로 이루어져 있었다. 항상 보면서 저게 뭔소린가 했는데;; 기본적인 내용도 모르는데 블로그는 어떻게 만들었는지 신기할 따름이다...허허<br/><br/>
[__생활코딩__](https://opentutorials.org/course/3084)에 잘 정리되어 있어서, 이를 따라가며 공부했다. 기본 개념을 정리하고자 배운 내용들을 포스팅했다.<br/><br/>

# 1. HTML 이란?
<strong>H</strong>yper<strong>T</strong>ext <strong>M</strong>arkup <strong>L</strong>anguage의 앞자를 따서 <strong>HTML</strong>이다. 이름에서도 알 수 있다시피 <strong>Markup Language</strong>이다.<br/><br/>

# 2. Tag의 개념과 기본적인 Tag
가장 중요한것은 <Strong>Tag</Strong>의 개념인 것 같다. 지금 작성하는 .md의 경우 <Strong>Markdown</Strong> 문법을 <Strong>HTML</Strong>로 변경하기 때문에 나타나지는 않지만, 다음과 같은 몇 가지 Tag가 있었다.<br/>

1. \<h1>, \<h2>, \<h3> : <Strong>Header</Strong>, 제목을 의미한다. Markdown에서는 #, ## 등으로 활용하는 것과 같은 의미인 듯 하다.
2. \<u> : <u>Underline</u>을 의미한다. 밑줄을 긋는다. Markdown의 <\_ ... \_>이 HTML의 \<u>로 번역되는 것 같다.
3. \<strong> : <strong>Bold</strong>를 의미한다. 마찬가지로 Markdown의 <\__ ... \__>가 HTML의 \<strong>로 변환되는 듯.
4. Tag를 닫을 때는 /를 붙인다. \<h1> ... \</h1>와 같이 활용하고, 다른 언어들과 마찬가지로 시작이 있으면 끝이 있어야 한다.

위와 같은 기본적인 Tag들 처럼, 이미 정형화된 형식이 있다. 형식에 맞춰 웹 페이지를 제작하는 것이 여러 모로 효율적이라는 결론.<br/><br/>

# 3. Tag에 사용되는 속성과 상속
## 1. Tag의 속성
- <Strong>속성</Strong> : Tag의 상세한 정보를 지정해주는 것.

위의 기본적은 Tag들은 속성이 필요하지 않은 간단한 Tag 이지만, 속성이 필요한 Tag도 존재한다.<br/><br/>
대표적인 예로 \<img>태그가 있다.<br/>
> \<img src="/IMAGE" width="100"><br/>

위와 같은 Image Tag를 보면, src라는 속성에서 "/IMAGE"라는 Image의 경로를 설정해주고, width라는 속성에서 100px의 가로 길이를 갖는다는 것을 명시해준다.

## 2. 상속
여러 프로그래밍 언어들과 마찬가지로, HTML에도 <Strong>상속</Strong> 개념이 존재한다. 아래의 코드를 입력하면 다음과 같이 나타난다.<br/><br/>
Input<br/>
<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#010101">&lt;</span><span style="color:#066de2">ol</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#010101">&lt;</span><span style="color:#066de2">strong</span><span style="color:#010101">&gt;</span>오늘&nbsp;할&nbsp;일<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">strong</span><span style="color:#010101">&gt;</span><span style="color:#010101">&lt;</span><span style="color:#066de2">br/</span><span style="color:#010101">&gt;</span><span style="color:#010101">&lt;</span><span style="color:#066de2">br/</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">li</span><span style="color:#010101">&gt;</span>&nbsp;라면&nbsp;사기&nbsp;<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">li</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">li</span><span style="color:#010101">&gt;</span>&nbsp;블로그&nbsp;포스팅&nbsp;<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">li</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#010101">&lt;</span><span style="color:#066de2">li</span><span style="color:#010101">&gt;</span>&nbsp;저녁&nbsp;챙겨먹기&nbsp;<span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">li</span><span style="color:#010101">&gt;</span></div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#010101">&lt;</span><span style="color:#010101">/</span><span style="color:#066de2">ol</span><span style="color:#010101">&gt;</span></div></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>

---

Output<br/>
<ol>
<strong>오늘 할 일</strong><br/><br/>
	<li> 라면 사기 </li>
	<li> 블로그 포스팅 </li>
	<li> 저녁 챙겨먹기 </li>
</ol>

---

이처럼 \<ol> Tag 안에 \<li> Tag가 존재하는데, 이런 경우엔
1. \<li> Tag가 \<ol> Tag의 Child Tag이고,
2. \<ol> Tag가 \<li> Tag의 Parent Tag이다.

위와 같이 표현할 수 있다. Java로 치면 _Interface_ 의 상속 개념이라기 보다는 _Project_, _Package_, _Class_ 개념과 더 가까운 것 같다.<br/><br/>

# 4. 가장 많이 활용되는 Tag들
HTML 문법과 Tag들은 상당히 가까이에 있는데, Chrome에서 F12를 누르면 개발자 도구를 볼 수 있다. 기본 뼈대가 되는 Tag들은 다음과 같다.

## 1. \<head>, \<title>, \<body>, \<html> Tag

1. \<head> : Header를 작성할 때 사용한다. Markdown에서 \--\- ... \--\- 에 Header를 작성하는 것과 같은 기능인 듯 하다.

2. \<title> : 웹 페이지의 제목을 작성할 때 사용한다. 몰랐지만, title을 잘 작성해야 Google 등의 엔진에서 효율적으로 크롤링할 수 있다고 한다. 이제부터라도 성의껏 잘 작성해야겠다.

3. \<body> : 본문의 내용을 적을 때 사용한다.

4. \<html> : \<head>와 \<body>를 감쌀 때 사용한다. 아마도 Java의 Package와 같은 구조인 듯 하다.(?)

## 2. \<meta> Tag

* \<meta> : Metadata를 지정할 때 사용한다. \</meta>가 필요하지 않다는 특징이 있다.

\<meta>태그는 상당히 다양한 용도로 쓰이는 듯 해서 따로 뺐다. 몇 가지 예를 들어보면 다음과 같다.

1. \<meta charset="utf-8"> : 해당 웹의 인코딩 방식을 UTF-8로 설정할 수 있다.
2. \<meta name="keyword" content="HTML"> : 해당 웹의 keyword를 설정할 수 있다. "HTML"로 설정한 예.
3. \<meta name="description" content="HTML description"> : 해당 웹에 대한 설명을 설정할 수 있다.
4. \<meta name="author" content="bye0nys"> : 해당 웹의 저자를 설정할 수 있다.
 
<br/>

이외에도 CSS를 불러오기 위한 \<link>, Hyperlink를 걸 수 있는 \<a> 등 여러 Tag가 있다.<br/><br/>

# 5. 배운 점

오늘 처음 배웠지만, 기본적인 문법은 다른 언어들과 상당히 비슷한 것 같다. 크게 느낀 점은 아래의 두 가지.

1. HTML의 숙련도는 <Strong>Tag</Strong>의 활용에 따라 갈리는 듯 하다.
2. 다른 언어와 마찬가지로, 중복되는 부분엔 _class_ 나 _id_ 를 선언해 정리하는 것이 편리할 것 같다.

<br/>
HTML 이외에도 디자인과 관련된 언어인 <Strong>CSS</Strong>가 존재한다. CSS 정리를 마치면 바로 <Strong>Javascript</Strong>를 공부해야겠다.