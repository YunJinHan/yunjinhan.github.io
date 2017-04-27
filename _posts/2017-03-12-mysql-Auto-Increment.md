---
layout: post
title: "MySQL - auto_increment"
date: 2017-03-12 +0900
tags: Problems
description: MySQL - auto_increment
---


MySQL - auto_increment
------------
- auto_increment 속성은 항상 Primary Key 로 지정되어야 한다.
- 시작 값은 Default 값으로 0 부터 주어진다.
- 중간 값이 삭제되더라도 삭제된 auto_increment 값이 새로 사용되지는 않는다.
- 마지막 값이 삭제된다면 해당 auto_increment 값 부터 시작한다.
<br>
#오류 발생 Query
<pre>
CREATE TABLE table (
	num int auto_increment
);
</pre>
<br>
#정상 Query
<pre>
CREATE TABLE table (
	num int auto_increment primary key
);
</pre>
<br>
위와 같은 문법적 오류를 발생시키는 원인은 auto_increment 속성이 유니크한 키 값을 자동으로 생성해주는 명령어로서 테이블 내에서 유니크한 키 값을 제외한 다른 primary key 를 선언할 수 없게 문법적으로 막아놓은 것이다.
