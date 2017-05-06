---
layout: post
title: "Node js - Asynchronous Programming"
date: 2017-05-07 +0900
tags: NodeJS
description: Node js 비동기 프로그래밍
share: true
comments: true
---

Node js 비동기 프로그래밍
====
### - Multi-Thread 의 한계
&nbsp;Multi-Thread 기반의 서버는 일반적으로 클라이언트의 요청마다 Thread 를 생성시킨다. 그렇기 때문에 요청이 많아지면 Thread 가 많아지며 그만큼 메모리 및 서버 전체에 영향을 크게 미치게 되며 공유자원 사용시 동기화 없이 접근한다면 예기치 못한 결과가 나올 수 있다.

### - 비동기 처리
&nbsp;동기 처리 방식은 하나의 요청이 처리되는 동안 다른 요청이 처리되지 못하며 해당 요청이 완료되어야 다음 요청이 처리되는 방식이다.

&nbsp;비동기 처리 방식은 하나의 요청 처리가 완료되기 전에 제어권을 다음 요청으로 넘겨 처리하는 방식이다. 이 경우 I/O 처리인 경우 Blocking 되지 않으며 다음 요청을 처리할 수 있는 것이다.

**Node js 는 비동기 처리방식으로 병렬처리를 하며,<br>Multi Thread 방식의 문제점을 보완하기 위해 Single Thread + Non-blocking I/O 방식을 도입한 FrameWork이다.**

## - Node js 의 비동기 처리
![screenshot]({{ site.url }}/assets/nodejsAsy.png)

**Event Loop는 NodeJS의 싱글 쓰레드에서 돌아가며 I/O Bound 작업들을 비동기적으로 처리해주기 위해서 필요하다.**<br>

**Client에서 I/O Bound 요청이 온다면 이 요청들은 Message 형태로 Event Queue에 저장된다.**<br>

**Event Loop는 Event Queue에 있는 Task들을 Pop하여 Non-Blocking 방식으로 Kernel에 처리를 요청하며 작업이 끝난 Task들을 감지하고 Callback function을 호출한다.**<br>

&nbsp;Node js 의 비동기 처리는 이벤트 방식으로 처리된다.<br>
클라이언트의 요청을 비동기로 처리하기 위해 이벤트가 발생하며 서버 내부에 메세지 형태로 전달된다. 이를 서버 내부에서 Event Loop 가 처리한다. Event Loop 가 처리하는 동안 제어권은 다음 요청으로 넘어가고 처리가 완료되면 Callback 함수를 호출하여 처리완료를 호출측에 알린다.

&nbsp;Event Loop는 요청을 처리하기 위하여 내부적으로 약간의 Thread와 프로세스를 사용하며, 이는 Non-Blocking IO 또는 내부 처리를 위한 목적으로만 사용되지 요청 처리 자체를 Thread로 하지는 않는다. 따라서 Node 서버는 Multi-Thread 방식의 서버에 비하여 Thread 수와 오버헤드가 훨씬 적다.

&nbsp;이벤트를 처리하는 Event Loop는 Single-Thread 로 이루어져 있다. 즉 요청 처리는 하나의 Thread 안에서 처리된다는 의미이다. 그래서 이벤트 호출 측에는 비동기로 처리되지만 처리작업 자체가 오래 걸린다면 전체 서버 처리에 영향을 주며, 이는 Node js 의 치명적인 약점이라고 볼 수 있다.

### - Node js 올바른 사용

&nbsp;Node js 는 Google Chrome V8 엔진 기반으로 동작하며 내부의 Event Loop는 Single-Thread 기반에서 비동기 메시지를 처리하며, 이러한 Event Loop는 고성능의 병렬처리를 보장하도록 설계되어 있다. 따라서 이벤트에 의해 처리해야 할 단위 작업이 아주 짧은 시간 안에 처리된다면 Node js의 고성능의 장점을 극대화 할 수 있다.

&nbsp;처리 작업이 CPU를 많이 소모한다든지 대용량 파일을 처리하는 작업보단 I/O 작업이 별로 없는 애플리케이션이나 단위작업이 짧은 메시징 애플리케이션의 경우에 Node js 의 성능이 제대로 발휘 될 수 있다.

&nbsp;따라서 Node 애플리케이션은 가능한 한 전부 비동기로 처리해야 하며 Node 개발자는 비동기 프로그래밍 방식에 익숙해져야 할 필요가 있다.

<br><br>

Asynchronous Programming Example
-----
**비동기적 코드 예제**
{% highlight javascript %}
var fs = require('fs');
console.log("I'm First!");
// practice.txt에는 "I'm Second!"라는 문장이 적혀있다.
fs.readFile("./practice.txt", (err, res) => { 
  console.log(res);
});
console.log("I'm Third!");
{% endhighlight javascript %}

**출력 결과**
{% highlight javascript %}
I'm First!

I'm Third!

I'm Second!
{% endhighlight javascript %}

동기적 코드처럼 순서대로 처리되지 않으며, callback 함수가 언제 끝나고 시작될지는 모른다.
<br>

**동기적 코드**
{% highlight javascript %}
function regist(user) {
	var shopUser = findShopUser(user);
	if(shopUser != null) {
		return 'already';
	}
	var result = registShopUser(user);
	if(result == true) {
		return 'success';
	} else {
		return 'fail';
	}
}
{% endhighlight javascript %}

**비동기적 코드**
{% highlight javascript %}
function regist(user, callback) {
	var shopUser = findShopUser(user, function(shopUser) { // callback1
		if(shopUser != null) {
			callback('already');
		}
		registShopUser(user, function(result) { // callback2
			if(result == true) {
				callback('success');  
			} else {
				callback('fail');
			}  
		}) ; 
	});
}
{% endhighlight javascript %}

**호출 가능한 callback 함수를 등록하여 I/O시간동안 대기하지 않고 바로 함수가 완료되며 완료되는 시점에 callback 함수가 호출된다. 해당 callback 함수의 파라미터값에 조회된 내용이 넘어온다.**

### - CallBack Hell
&nbsp;이러한 비동기적인 코드를 작성하다보면 언제 I/O Bound Task들이 끝나고 callback 함수가 호출됐는지 알 수 없다. 그래서 우리는 동기적인 코드가 필요할 때 task가 끝나면 반드시 호출되는 callback들을 이용해야한다.<br>
&nbsp;즉 여러개의 I/O Bound Task를 동기적으로 처리하고 싶다면 callback 함수내에서 다른 I/O Bound Task를 호출해야하고 이러한 동기적 호출이 몇개나 더 필요하다면 callback 안에 callback 그리고 더많은 callback 들을 이용해야한다.<br>
&nbsp;이것을 **Callback Hell**이라고 부른다.

--> **함수들을 순차적으로 실행하고자 할때 중첩된 callback 을 사용하여 코드가 복잡해지고 가독성이 떨어져 유지보수가 매우 힘들어질수 있다**


Asyn
-----
이와 같은 복잡성을 해결하기 위해 Node js 에서는 대표적으로 **async** 라는 callback hell 문제를 해결하는 라이브러리가 제공된다.

20여 가지 정도의 기능이 있으나 대표적으로 **waterfall / series** 두개의 기능을 보자.

### - waterfall
{% highlight javascript %}
waterfall([tasks],callback)
{% endhighlight javascript %}

waterfall 에는 두가지 인자를 넘기는데,<br>첫번째 인자는 실행될 함수들을 배열로 정의한 배열 변수이며<br>두번째 변수는 모든 함수가 순차적으로 끝난후에 맨 마지막에 실행되는 함수이다. <br>또한 실행 도중 에러 발생시 최종 callback 을 호출한다.

**각각 task의 실행 결과가 다음 task의 파라미터로 전달된다.**

![screenshot]({{ site.url }}/assets/waterfall.png)

{% highlight javascript %}
var async = require('async');
async.waterfall(
	[
		function(callback){
			asyncfunctionA(param,callback);
		},
		function(resultA,callback){
			asyncfunctionB(resultA,callback);
		},
		function(resultB,callback){
			asyncfunctionC(resultB,callback);
		}
	],
	function(err,resultC){
		if(err) errorHandler(err);
		// handle resultC
	}
);
{% endhighlight javascript %}

**기본적인 구조이며, 각 콜백에서 넘긴 변수들이 마지막 완료 시점에서 리턴되는 것이 아니라 다음 작업 파라미터로 전달된다.**

**Example**

{% highlight javascript %}
var async = require('async');
var fs = require('fs');
var mysql = require('mysql');
var connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: '',
    database: 'movie'
});

var tasks = [
    function (callback) {
        connection.query('select * from movie where title=? and year=?;', ['인사이드 아웃', 2015], function (err, row) {
            if (err) return callback(err);
            if (row.length == 0) return callback('No Result Error');
            callback(null, row[0]);
        })
    },
    function (data, callback) {
        connection.query('SELECT * FROM review WHERE movie_unique_id=?;', [data.code], function (err, rows) {
            if (err) return callback(err);
            callback(null, rows);
        });
    },
    function (reviews, callback) {
        fs.writeFile('insideout.txt', JSON.stringify(reviews), function (err) {
            if (err) return callback(err);
            callback(null)
        });
    }
];

async.waterfall(tasks, function (err) {
    if (err)
        console.log('err');
    else
        console.log('done');
    connection.end();
});
{% endhighlight javascript %}

1. **데이터베이스의 movie 테이블에서 제목이 인사이드 아웃 인 영화의 코드를 찾는다.**
2. **1에서 찾은 영화 코드를 사용하여 review 테이블에서 인사이드 아웃 의 리뷰들을 찾는다.**
3. **2에서 찾은 리뷰를 insideout.txt로 저장한다.**


### - Series
{% highlight javascript %}
series([tasks],callback)
{% endhighlight javascript %}

series 또한 waterfall 과 마찬가지로 두가지 인자를 넘기는데,<br>첫번째 인자는 실행될 함수들을 배열로 정의한 배열 변수이며<br>두번째 변수는 모든 함수가 순차적으로 끝난후에 맨 마지막에 실행되는 함수이다. <br>또한 실행 도중 에러 발생시 최종 callback 을 호출한다.

**waterfall 과 다른점은 긱 task의 결과를 취합하여, 최종 callback에 배열 형태로 전달한다는 점이다.**

![screenshot]({{ site.url }}/assets/series.png)

{% highlight javascript %}
var async = require('async');

async.series(
	[
		function(callback){
			callback(null,'resultA');
		},
		function(callback){
			callback(null,'resultB');
		},
		function(callback){
			callback(null,'resultC');
		}
	],
	function(err,results){
		if(err) console.log(err);
		console.log(results);
	}
);
{% endhighlight javascript %}

출력결과

{% highlight javascript %}['resultA','resultB','resultC']{% endhighlight javascript %}

**Example**

{% highlight javascript %}
var async = require('async');

var tasks = [
    function (callback) {
        setTimeout(function () {
            console.log('one');
            callback(null, 'one-1', 'one-2');
        }, 200);
    },
    function (callback) {
        setTimeout(function () {
            console.log('two');
            callback(null, 'two');
        }, 100);
    }
];

async.series(tasks, function (err, results) {
    console.log(results);
    // [ ['one-1', 'one-2'], 'two' ]
});
{% endhighlight javascript %}

**tasks의 첫번째 함수에서는 0.2초 후에  one 이라는 메시지를 출력하고, 두번째 함수에서는 0.1초 후에 two 라는 메시지를 출력하도록 해놓았다. 이 함수를 async의 series 함수로 실행하면 작업 시간은 첫번째 함수가 실행된 후 두번째 함수가 실행되어 총 0.3초가 걸린다. 각 수행의 결과는 results에 배열 형태로 반환된다.**