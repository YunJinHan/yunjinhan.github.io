---
layout: post
title: "Cookie & Session"
date: 2017-04-03 +0900
tags: Problems
description: Cookie & Session
share: true
comments: true
---

Cookie
=============
데이터를 클라이언트상에 저장하는 방법

웹 사이트와 사용자의 컴퓨터를 매개해주는 정보를 담고있는 소량(4KB 이하)의 파일 (php 4.1 이상 가능)<br>
-> php 와 javascript 간에 정보 공유 가능.

Cookie Create
-------------
{% highlight javascript %}setcookie(string $name, string $value, int $time, string $path, string $domain, bool $secure = false, bool $httponly = false){% endhighlight javascript %}

1. $name : 쿠키 이름 ( 공백이나 마침표 안됨 , 대소문자 구분 )
2. $value : 쿠키 값
3. $expire : 쿠키 만료시간 ex) time() + 3600 -> 1시간
4. $path : 지정된 경로에서만 유효 쿠키로 사용 ex) '/' -> 사이트 전체 쿠키 노출
5. $domain : 지정된 도메인에 있을때만 유효 쿠키로 사용
6. $secure / $httponly : 값이 1 인 경우 https 를 통해서만 전송되야함


ex) 
PHP
쿠키 설정
{% highlight javascript %}setcookie('category', 'test1', time()+(60*60*24));{% endhighlight javascript %}


Cookie Load
------------
{% highlight javascript %}$_COOKIE['category']{% endhighlight javascript %}


Cookie Modify
------------
{% highlight javascript %}setcookie('category', 'test2', time()+(60*60*24));{% endhighlight javascript %}


Cookie Delete
------------
{% highlight javascript %}setcookie('category'){% endhighlight javascript %}


Javascript - Cookie Create
------------
{% highlight javascript %}document.cookie='category=' + cookie_value;{% endhighlight javascript %}

<br>
<br>
<br>

Session
============
데이터를 서버상에 저장하는 방법 ( Cookie 보다 안전하며 , 더 많은 데이터를 저장 )

Session Start
------------
{% highlight javascript %}session start();{% endhighlight javascript %}
세션 이용하기 전에 반드시 호출해야하며 호출 전에 어떤 내용도 브라우저에 출력하면 안된다.


Session Create
------------
{% highlight javascript %}$_SESSION['category']{% endhighlight javascript %}

Session Delete
------------
특정 세션변수만 삭제
{% highlight javascript %}unset($_SESSION['category']){% endhighlight javascript %}

전체 세션변수 삭제
{% highlight javascript %}$_SESSINO = array();{% endhighlight javascript %}

서버에 저장된 모든 세션 데이터 삭제
{% highlight javascript %}session_destory();{% endhighlight javascript %}