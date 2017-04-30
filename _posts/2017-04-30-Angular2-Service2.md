---
layout: post
title: "Angular2 Service2"
date: 2017-04-30 10:00 +0900
tags: Angular2
description: Angular2 Service - promise / shared data
share: true
comments: true
---

Angular2 Service2
=====

### - Promise 서비스
promise 는 비동기 코드를 사용할 때 Call Back Hell 과 같은 비정상적인 호출 형태를 개선하기 이해 나온 방법이다.

<pre>
export class User {
  name: string;
  id: string;
}
</pre>

===> user.ts

<pre>
import { User } from './user';

export var USERS: User[] = [
  { name: '조조', id: '1' },
  { name: '하우돈', id: '2' },
  { name: '허저', id: '3' }
];
</pre>

===> mock-user.ts

<pre>
import { Injectable } from '@angular/core';
import { USERS } from './mock-user';
import { User } from './user';

@Injectable()
export class MockService {
  getUser(): Promise< User[]> {
    return Promise.resolve(USERS);
  }

  getUserDelay(): Promise< User[]> {
    return new Promise< User[]>(resolve =>
      setTimeout(resolve, 1000))
      .then(() => this.getUser());
  }
  
  getRequest(status: boolean): Promise< any> {

    if (status) {
      return Promise.resolve("요청을 승낙합니다.").then(function (reason) {
        return reason;
      }, function (reason) {
        return "NO";
      });
    } else {
      return Promise.reject("요청을 거부합니다.").then(function (reason) {
        return "YES";
      }, function (reason) {
        return reason;
      });
    }
  }
}
</pre>

===> mock.service.ts<br>**Promise.resolve()** 를 통하여 "요청을 승낙합니다" 를 리턴하고<br>**Promise.reject()** 를 통하여 "요청을 거부합니다" 를 리턴한다.

**getUser()** 는 mock-user.ts 의 USERS 객체를 리턴한다.<br>
**getUserDelay()** 를 통하여 1초 후 **getUser()** 를 호출한다.

<pre>
import { Component } from '@angular/core';
import { MockService } from './mock.service';
import { User } from './user';

@Component({
  selector: 'promise',
  template: `
  {{reqMessage}}< br>
  {{reqMessage2}}< br>  
  < list-component [list]="listUser" [title]="'이름 출력 (지연없음)'">< /list-component>
  < list-component [list]="listUserDelay" [title]="'이름 출력 (1초 지연)'">< /list-component>`,
  providers: [MockService]
})
export class PromiseComponent {
  reqMessage: String ='';
  reqMessage2: String ='';

  listUser: User[];
  listUserDelay: User[];
  
  constructor(private userService: MockService) {
    this.userService.getRequest(true).then(reason => this.reqMessage = reason);
    this.userService.getRequest(false).then(reason => this.reqMessage2 = reason);
    // -> reason 을 리턴받아 => this.regMessage 에 할당한다.

    this.userService.getUser().then(user => this.listUser = user);
    this.userService.getUserDelay().then(user => this.listUserDelay = user);
  	// -> user 를 리턴받아 => this.listUser 에 할당한다.
  }
}
</pre>

===> promise.component.ts<br>생성자 부분에서 mock.service 를 선언하고 해당 서비스를 사용하여 값을 받아와<br>자식 컴포넌트 list-component 로 title 과 list 값을 전달한다.

<pre>
import { Component, Input } from '@angular/core';
import { User } from './user';

@Component({
  selector: 'list-component',
  template: `
  < b>{{title}}< /b>
  < div *ngFor="let o of list">
      {{o.id}} | {{o.name}}
  < /div>< br>`,
})
export class ListComponent {
  @Input() title: string;
  @Input() list: User;
}
</pre>

===> list.component.ts / 부모로 부터 받은 title 과 list 를 출력한다.

### - Shared 서비스
서비스는 컴포넌트 간 상호작용을 위한 매개자로 두 컴포넌트 간에 데이터를 교환하는 중간 매개자로 이용될 수 있다. **@Input 이나 EventEmitter 를 사용하지 않고 데이터를 교환하려면 부모 / 자식 컴포넌트 관계를 가져야 한다.**

1. 부모 컴포넌트에 제공자 설정을 통해 주입을 한다.
2. 자식 컴포넌트에서는 제공자 설정을 하지 않고 곧바로 서비스를 받아서 사용한다.

<pre>
import { Injectable } from '@angular/core';

@Injectable()
export class SharedService {
  
  num: number;
  
  message: string;
  // -> 현재 실습에서 사용하는 부모 / 자식 컴포넌트 공유 변수
  
  public names: string[] = [];

  addName(data: string) {
    this.names.unshift(data);
    // unshift : names 배열 앞부분에 data 값을 넣는다
  } 
}
</pre>

===> shared.service.ts / 공유 서비스 정의

<pre>
import { Component, Input } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'car-component',
  template: `car 컴포넌트 : {{ s.message}} < button (click)="s.message='car'">선택< /button>`
})
export class CarComponent {
  constructor(public s: SharedService) {
  }
}
</pre>

===> car.component.ts / 자식 컴포넌트

<pre>
import { Component, Input } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'taxi-component',
  template: `taxi 컴포넌트 : {{ s.message}} < button (click)="s.message='taxi'">선택< /button>`
})
export class TaxiComponent {
  constructor(public s: SharedService) {}
}
</pre>

===> taxi.component.ts / 자식 컴포넌트

<pre>
import { Component, Input } from '@angular/core';
import { SharedService } from './shared.service';

@Component({
  selector: 'parent-component',
  template: `
  부모 컴포넌트 : {{s.message}} < button (click)="s.message='parent'">선택< /button>< br>
  < car-component>< /car-component>< br>
  < taxi-component>< /taxi-component>`,
  providers: [SharedService]
})
export class ParentComponent {
  constructor(public s: SharedService) {
    s.message = "hello";
  }
}
</pre>

===> parent.component.ts / 부모 컴포넌트


두개의 자식 컴포넌트의 생성자에서 SharedService 를 선언하여 s.message 를 불러오고

부모 컴포넌트 생성자에서 SharedService 를 선언하여 처음 s.message 를 hello 로 선언함

이후 자식에서 버튼 클릭시 s.message 가 각각 car / taxi 로 바뀌어 s.message 를 사용하는

parent / car / taxi 컴포넌트에서 해당 값이 실시간으로 바뀐다.

**공유 서비스를 통해 공유 값이 바뀐다**



