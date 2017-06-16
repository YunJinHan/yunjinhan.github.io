---
layout: post
title: "MySQL - Join"
date: 2017-06-17 +0900
tags: MySQL
description: MySQL - Join
share: true
comments: true
---

SQL Join
====
Inner Join
----

### Equi Join (동등 조인)
- 두개의 테이블을 비교하여 같은 데이터만 불러오는 교집합

Example)
<pre>
< Table A >
a b
- -
1 a
2 b
3 c
< Table B >
a b c
- -
2 b b
3 c e
< Output Table >
a  b b c
2  b b b
3  c c e
</pre>
Query1 : 
**select A.a, A.b, B.c from A inner join B on A.a = B.a;**<br>Query2 : **select A.a, A.b, B.c from A, B where A.a = B.a;**

### Natural Join (자연 조인)
- 동등 조인과 거의 유사하나 같은 이름을 가진 컬럼은 한 번만 추출한다.

<pre>
< Table A >
a b
- -
1 a
2 b
3 c
< Table B >
a b c
- -
2 b b
3 c e
< Output Table >
a  b c
2  b b
3  c e
</pre>
Query : 
**select A.a, A.b, B.c from A natural join B;**

### Cross Join (크로스 조인 or Cartesian Product)
- 조인에 참여한 테이블들의 모든 데이터가 추출된다.

Query1 : **select * from A cross join B;**<br>Query2 : **select * from A, B;**

Outer Join
----

### Left Outer Join
- 왼쪽 테이블을 기준으로 두개의 테이블을 비교하여, 왼쪽의 데이터가 오른쪽 테이블에 없으면 NULL 로 표현된다.

Example)
<pre>
< Table A >
a b
- -
1 a
2 b
3 c
< Table B >
a c
- -
2 b
3 e
< Output Table >
a  b 	c
1  a 	null
2  b 	b
3  c 	e
</pre>
Query : 
**select A.a, A.b, B.c from A left outer join B on A.a = B.a;**

### Right Outer Join
- 오른쪽 테이블을 기준으로 두개의 테이블을 비교하여, 오른쪽의 데이터가 왼쪽 테이블에 없으면 NULL 로 표현된다.

Example)
<pre>
< Table A >
a b
- -
1 a
2 b
3 c
< Table B >
a c
- -
2 b
3 e
< Output Table >
a  b 	c
2  b 	b
3  c 	e
</pre>
Query : 
**select A.a, A.b, B.c from A right outer join B on A.a = B.a;**


Inener / Outer Join 의 활용
=====
![screenshot]({{ site.url }}/assets/join.png)