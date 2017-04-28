---
layout: post
title: "Angular2 Outline"
date: 2017-04-06 +0900
tags: Angular2
description: Angular2 Outline
share: true
comments: true
---

Angular2
==========
- Component 기반 개발<br>
Component 기반의 개발로 생산성을 높이며, 대규모 애플리케이션에 걸맞는 구조를 만든다.<br>
Angular1 의 Controller 기능을 완전히 대체하며 구조적으로 Controller 보다 견고하다.<br>
- TypeScript 주력 언어<br>
- 고성능 프레임워크<br>
Lazy Loading 사용 <br>==> 애플리케이션을 실행하는 시점에 모든 Module 을 로딩하지 않고 현재 페이지에 필요한 Module 만 로딩하는 방식<br>
AoT 컴파일 사용 <br> ==> HTML Template 과  CSS 파일을 컴파일해 코드로 삽입하는 방식

Angular2 Architecture
----------

![srceenshot]({{ site.url }}/assets/angular_architecture.png)

Angular2 는 **Component (컴포넌트)** 중심으로 개발된다. 

- **Component (컴포넌트)**
	- **Template (템플릿)** : Component 의 UI 를 나타내며 지시자와 템플릿 문법을 활용하여 개발한다.<br>해당 UI 는 Virtual DOM 에 저장되며 각각의 컴포넌트 마다 개별적인 Virtual DOM 을 이용하므로 서로간의 스타일에 영향을 받지 않는다.
	- **Class (클래스)** : Component 의 Logic 을 관리하며 Template 과 Class 는 **Binding (바인딩)** 을 통해 연결된다.

	
( ⚒ **Binding (바인딩)** 이란? : Template 에서 Class 로 이벤트를 전달하거나 서로 간의 데이터를 교환할 수 있게 하는것. 서로의 구성요소가 동일한 변수로 바인딩하고 있다면 엘리먼트의 상태를 변경하기 위해 구성요소마다 값을 변경할 필요가 없다. --> 상태변화가 일어난다면 감지후 자신의 상태를 변경함 )

- Component 마다 라우팅 URL 주소를 할당 할수 있다. ( 라우팅 : URL 의 변경 )
- Component 는 자식 Component 를 포함하거나 다른 Component 로 URL 을 라우팅할 수 있습니다.<br>자식 Component 를 포함할 떄는 자식 Component 의 지시자를 현재 Component 의 Template 에 입력함으로써 포함시킨다.
- Component 와 Component 는 라우팅을 통해 전환될 수 있다.
- Component 는 외부의 도움을 받아 기능을 더하거나 ( **Module (모듈)** 을 사용) 중복을 최소화 시킬 수 있다.
- Component 내부의 중복 Logic 코드를 최소화하는 방법으로 **Services (서비스)** 를 사용한다.<br> Services 를 Component 에서 제공할 때는 **Dependency Injection (의존성 주입)** 을 이용한다.<br>( Dependency Injection : Service 를 객체로 만들어 Class 에 제공하는 역할 )

Angular2 구성요소
------------
### - Component ( 컴포넌트 )
<pre>
import { Component } from '@angular/core';

@Component({
	selector : 'my-component',
	template: '{{msg}}'
})

export class MyComponent {
	msg : string = "hello";
}
</pre>
Component 를 정의할 때는  @Component 를 이용하여 정의한다.<br>Selector 속성은 Component 의 지시자가 위치하며 Template 속성에는 해당 Template UI 가 위치한다.<br>
Class 에는  Component 에서 사용할 Logic 이 위치한다.<br> 
===> Component 는  독립적으로 동작하지 않고  Module 이나 Service, Directive 와 함께 동작합니다.
<br>
### - Module ( 모듈 )
<br>
<pre>
export class Hello{}
import { Hello } from './hello';
</pre>
Module 에는 Angular 라이브러리 Module 과 사용자가 정의하는 Module 이 있다.<br>
Module 을 정의하려면 export 키워드를 사용하여 Module 을 정의한다.

Angular 에서는 Module 을 체계적으로 구성하기 위해서 Class 선언 위에 장식자를 추가한다.<br>
장식자가 추가된 Angular 의 Module 로  Component, Directive, Pipe 가 있다.<br>

허나, Component 를 기반으로  Module 을 구성하는 과정은 체계적이지 않고 복잡하기 때문에<br> **@NgModule** 장식자를 이용하여 일반적인 Module 을 구성한다.

<pre>
@NgModule({
	import: [
		Angular 모듈 , Routing 모듈, ...
	],
	declarations: [
		Componennt, Directive, Pipe, ...
	],
	providers : [
		Service Module, ...
	]
})
export class MyModule {}
</pre>
&nbsp;<br>
### - Service ( 서비스 )
<br>
<pre>
import { Injectable } from '@angular/core';
export class HelloService {}
</pre>
Service 는  Component 에 제공할 목적으로 외부에 정의한 Class 이며 재사용 가능한 Logic 을 정의하기 위한 용도로 Component 외부에 정의하며 직접적으로 Component 와 관련이 없는 Logic 은  Service 를 통하여 정의한다.<br>

**@Injectable** 장식자를 이용하여 Service 를 선언하고 위와 같이 정의한다.

Service 를 이용하면 중복 Logic 을 제거할 수 있고, Component 내부의 관심사를 외부로 분리하는 효과를 줄수 있다.
<br>
### - Directive ( 지시자 )
<br>
<pre>
@Component({
	selector : 'my-component',
	template : '<   div helloStyle><   /div>'
})
</pre>
==> helloStyle 이란 Directive 가 Template 에 표현되는 방법<br>
Angular 는 선언형 프로그래밍을 권장하며 선언형 프로그래밍은 Directive 를 이용해 개발하는 것을 의미한다.
Directive 는 Component 의 Template 에서 Element 의 속성과 같이 구성요소의 속상이나 이름으로 표현한다.


Directive 를 이용하면 UI logic 코드를 반복적으로 입력할 필요가 없다. 그 이유는 Template 출력과 관련된 기능을 Directive 를 통해 logic 을 분리할 수 있기 떄문이다.

Directive 는 화면 출력시 필요한 기능을 외부 Module 로 분리해 정의하고  **@Directive** 장식자를 이용해 정의한다.

<pre>
import { Directive, ElementRef, Renderer } from '@angular/core';
@Directive({
	selector : '[helloStyle]'
})
export class HelloStyleDirective {
	constructor(private el : ElementRef, privatae renderer : Renderer) {}
	...
}
</pre>
Angular 에서는 Template 에서 사용할 수 있는 활용성 높은 여러 내장 Directive 를 제공한다.