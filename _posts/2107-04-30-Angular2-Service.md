---
layout: post
title: "Angular2 Service1"
date: 2017-04-30 +0900
tags: Angular2
description: Angular2 Service - basic / oop / mock service
share: true
comments: true
---

Angular2 Service
====
서비스는 **@Injectable** 장식자 ( = 주입 가능한 클래스라는 뜻 ) 를 추가한 클래스 이다.<br>
@Injectable 을 생략한다고 해서 의존성 주입에 문제가 생기는 것은 아니지만, 생략하였을때 일반 클래스라 오인할 수 있기 때문에 반드시 추가해야 한다.

{% highlight javascript %}
import { Injectable } from '@angular/core';

@Injectable()
export class HelloService {
  sayHello(){
    return "Hello 서비스!";
  }
  constructor() {}
}
{% endhighlight javascript %}

===> hello.service.ts / HelloService 라는 서비스 생성

{% highlight javascript %}
import { Component } from '@angular/core';
import { HelloService } from './hello.service';

@Component({
  selector: 'hello',
  template: `{{welcome}}`,
  providers: [HelloService]
  // -> providers 로 서비스 선언한다.
})
export class HelloComponent {
  welcome: string;

  constructor(helloService: HelloService) {
    this.welcome = helloService.sayHello();
  } // -> 생성자에서 서비스 선언 후 사용 (추천)
  
  constructor() {
  	let Hello = new HelloService();
  	this.welcome = Hello.sayHello();
  } // -> 생성자에서 서비스 선언하지 않고 사용 (비추천 : 새로운 변수 생성 / 메모리 사용)
{% endhighlight javascript %}

===> hello.component.ts<br>providers 에 사용할 서비스를 컴포넌트에서 HelloService 를 불러와 생성자에서 선언 후 사용.<br>
생성자에서 선언하지 않고 ***let Hello = new HelloSerivce();*** 로 사용가능.

### - 객체지향 서비스
크게 부모 / 자식 클래스가 있고 자식 클래스에는 공통 메서드가 정의돼 있기 떄문에 공통 메서드를 제어할 수 있는 인터페이스를 정의한다.

{% highlight javascript %}
import { Injectable } from '@angular/core';

@Injectable()
export class Parent {

  getName(){    
    return "Parent 서비스!";
  }
}
{% endhighlight javascript %}

===> parent.service.ts / 부모 서비스

{% highlight javascript %}
import { Injectable } from '@angular/core';
import { Parent } from './parent.service';

export interface Child {
  getData();
}

@Injectable()
export class FirstChild implements Child {

  constructor(public parent: Parent) { }
  // -> Child 인터페이스만 사용하였기 떄문에 부모는 생성자에 선언하여 사용하여야 한다.
  
  getData() {
    return [
      { Child: 'FirstChild 서비스' },
      { parent: this.parent.getName() }
    ];
  }
}

@Injectable()
export class SecondChild extends Parent implements Child {
  getData() {
    return [
      { Child: 'SecondChild 서비스' },
      { parent: super.getName() }
      // -> 부모를 상속하였기 때문에 super 로써 사용가능하다.
    ];
  }
}
{% endhighlight javascript %}

===> child.service.ts / 자식 서비스<br>
위의 예시처럼 First / Second 로 Parent 를 사용가능하지만<br>
***SecondChild 에서는 super 를 통하여 호출하기 때문에 다른 컴포넌트나 객체로 부모의 데이터를 전달 할수 없으나 FirstChild 에서 처럼 생성자에서 부모를 선언 후 사용한다면 전달이 가능하기 떄문에 extends 를 사용하는것 대신에 생성자에서 부모를 선언하여 사용하는 것이 바람직하다.***

{% highlight javascript %}
import { Component } from '@angular/core';
import { Parent } from './parent.service';
import { FirstChild, SecondChild } from './child.service';

@Component({
  selector: 'oop-cmp',
  template: `
      생성자 주입방식<br>
      {{firstChildData | json}}<br>
      상속방식<br>
      {{secondChildData | json}}`,
  providers: [Parent, FirstChild, SecondChild]
})
export class OopComponent {
  firstChildData;
  secondChildData;
  
  constructor(firstChild: FirstChild, secondChild: SecondChild) {
    this.firstChildData = firstChild.getData();
    this.secondChildData = secondChild.getData();
  }
}
{% endhighlight javascript %}

===> oop.component.ts / 컴포넌트

### - 목 객체 서비스
서버 의존성 없이 데이터를 제공하기 위해 테스트 데이터를 정의한 객체이다.<br>
목 객체를 사용하는 이유는 개발 과정에서 서버 의존성과 같은 외부 의존성을 제거함으로써 테스트를 가능하게 하기 위함이다.

{% highlight javascript %}
export class User {
  name: string;  
  id : string;
}
{% endhighlight javascript %}

===> user.ts / User 데이터 객체

{% highlight javascript %}
import { User } from './user';

export var USER: User[] = [
  {name: '유비',id:'1'},
  {name: '관우',id:'2'},
  {name: '장비',id:'3'}  
];
{% endhighlight javascript %}

===> mock-user.ts / 테스트 데이터 객체 USER 생성

{% highlight javascript %}
import { Injectable } from '@angular/core';
import { USER } from './mock-user';

export interface DataServiceStructure{
  getUser();
}

@Injectable()
export class MockService implements DataServiceStructure{
  constructor() {}
  getUser(){
    return USER;
  }
}
{% endhighlight javascript %}

===> mock.service.ts / 목 서비스 생성 : 테스트 데이터 USER 를 리턴하는 서비스 생성

{% highlight javascript %}
import { Component, OnInit } from '@angular/core';
import { MockService } from './mock.service';
import { User } from './user';

@Component({
  selector: 'mock',
   template: `
    <b>이름 출력</b>
    <div *ngFor="let o of listUser">
        {{o.id}} | {{o.name}}
    </div>`,
  providers: [MockService]
})
export class MockComponent {

  listUser: User[];
  // -> listUser 라는 리스트를 해당 User 라는 데이터 타입의 객체로 사용한다.

  constructor(private userService: MockService) {
    this.listUser = userService.getUser();
    // -> MockService 를 선언하여 해당 USER 데이터를 불러와 저장함. 
  }
}
{% endhighlight javascript %}

===> mock-component.ts / 해당 서비스와 데이터 타입 객체를 불러와 사용