---
layout: post
title: "Angular2 Core Module"
date: 2017-05-07 +0900
tags: Angular2
description: Angular2 Core Module
share: true
comments: true
---

Angular2 Core Module
====
핵심 모듈은 Angular 애플리케이션 관점에서 핵심이 되는 모듈로, 애플리케이션 루트 모듈에 한번 설정함으로써 전역에서 사용할 수 있는 모듈을 말한다. 루트 모듈에 등록됐다는 것은 애플리케이션이 시작될 때 처음 한 번만 호출해서 전역으로 사용하겠다는 의미이다.

대표적인 예로 **타이틀컴포넌트**가 있다.

#### - title component
애플리케이션 실행 시 호출되어 애플리케이션이 실행되는 동안 항상 사용된다.

아래의 예제에서는 컴포넌트와 서비스를 핵심 모듈로 등록해서 핵심 모듈을 구성해 보겠다.<br>
핵심 모듈에 등록할 title component 는 다음과 같이 정의하며 편의상 대부분 app/core 디렉토리에 정의한다.

{% highlight javascript %}
import { Component, Input } from '@angular/core';
import { UserService } from '../core/user.service';

@Component({
  selector: 'app-title',
  template: `
  <h1 highlight>{{title}}</h1>
  by <b>{{user}}</b>`
})
export class TitleComponent {
  @Input() title = '';
  user = '';

  constructor(userService: UserService) {
    this.user = userService.nickName;
  }
}
{% endhighlight javascript %}

===> title.component.ts

이이서 핵심 모듈에 등록할 UserService 는 다음과 같다.

{% highlight javascript %}
import { Injectable, Optional } from '@angular/core';

export class UserServiceConfig {
  nickName = '';
}

@Injectable()
export class UserService {
  private _nickName = '';

  constructor( @Optional() config: UserServiceConfig) {
    if (config) { this._nickName = config.nickName; }
  }

  get nickName() {
    return this._nickName;
  }
{% endhighlight javascript %}

===> user.service.ts

Userservice 서비스는 애플리케이션 루트 모듈에 등록되기 떄문에 싱글턴으로 존재하며, 전역 서비스로 사용이 된다.<br>
**위의 생성자에서 @Optional 장식자를 이용해 주입객체가 있다면, 해당 객체를 받게하고, 없다면 null 값을 반환한다.**<br>
**그리고 주입 객체에 대한 조건문 검사를 통해 객체가 있다면 서비스에 선언된 _nickName 프로퍼티를 갱신한다.**


마지막으로 핵심 모듈 설정은 다음과 같이 한다.

{% highlight javascript %}
import { ModuleWithProviders, NgModule, Optional, SkipSelf } from '@angular/core';
import { CommonModule } from '@angular/common';

import { TitleComponent } from './title.component';
import { UserService } from './user.service';
import { UserServiceConfig } from './user.service';

@NgModule({
  imports: [CommonModule],
  declarations: [TitleComponent],
  exports: [TitleComponent],
  providers: [UserService]
})
export class CoreModule {
  constructor( @Optional() @SkipSelf() parentModule: CoreModule) {
    if (parentModule) {
      throw new Error(
        'CoreModule이 이미 로딩되었습니다.');
    }
  }

  static forRoot(config: UserServiceConfig): ModuleWithProviders {
    return {
      ngModule: CoreModule,
      providers: [
        { provide: UserServiceConfig, useValue: config }
      ]
    };
  }
}
{% endhighlight javascript %}

===> core-module.ts

핵심 모듈을 애플리케이션 루트 모듈에 추가하기 위해 @NgModule 설정에서 TitleComponent 를 declarations에 선언하고 다시 외부로 노출하도록 exports에 선언하였다.

**생성자에서는 핵심 모듈인 자기 자신을 호출하여 부모 주입기에 핵심 모듈이 이미 생성되어 있는지 검사한다.**

**부모 주입기에 대한 의존성을 확인하기 위해 @SkipSelf 장식자를 이용하며, 부모 주입기에 객체 ( 자기자신 ) 이 존재하는지 확인 후 존재하면 해당 객체를 받기 위해 @Optional 장식자를 이용해 주입받는다**

**@Optional 장식자는 객체 존재시 해당 객체를 전달하고, 없다면 null 을 전달한다.**

**마지막으로 해당 핵심 모듈 설정을 전부 작성하였으면 export 로 노출 시킨 후 이를 AppRoot 모듈에 임포트 한다.**

{% highlight javascript %}
...
import { CoreModule } from './core/core.module';
...

@NgModule({
  imports: [
	 ...,
    CoreModule.forRoot({nickName: 'Happy'}),
    ...
  ],
	...
})
export class AppModule { }
{% endhighlight javascript %}

===> app-routing.module.ts

**forRoot 에 선언된 값 제공자를 받아 설정하기 위해 CoreModule.forRoot() 메서드를 호출해 값 제공자에 매개변수로 값을 전달한다.**

**이제 해당 핵심 모듈은 전역에서 사용 가능하게 등록이 되었다.**

{% highlight javascript %}
import { Component } from '@angular/core';

@Component({
  selector: 'core-test',
  template: `
  <app-title [title]="title"></app-title>`
})
export class CoreTestComponent {

  title = '반갑습니다! Core Module!';

}
{% endhighlight javascript %}

===> core-test.component.ts

테스트 컴포넌트는 루트 라우팅 모듈에 다음과 같이 등록한다.

{% highlight javascript %}
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
...
import { CoreTestComponent } from './core-test/core-test.component';

const appRoutes: Routes = [
  ...,
  { path: 'core-test', component: CoreTestComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
{% endhighlight javascript %}

===> app-routing.module.ts

테스트 컴포넌트에 대한 라우터 설정이 끝났고, http://localhost:4200/core-test 로 접속하면 다음과 같이 테스트 컴포넌트에 추가한 타이틀 컴포넌트가 실행된다.

<pre>
<h1 highlight>반갑습니다! Core Module!</h1>
by <b>Happy</b>
</pre>
