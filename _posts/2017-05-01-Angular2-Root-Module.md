---
layout: post
title: "Angular2 Root Module"
date: 2017-05-01 03:00 +0900
tags: Angular2
description: Angular2 Root Module
share: true
comments: true
---

Angular2 Root Module
====
애플리케이션에서 루트 모듈은 애플리케이션에서 최상위 모듈이며 애플리케이션 레벨의 서비스와 컴포넌트, 지시자, 파이프를 선언하거나 특징 모듈이라고 하는 다른 하위 모듈 설정을 포함하기도 한다.

Angular 애플리케이션은 최소한 하나 이상의 모듈 클래스를 갖는데, 이때 항상 정의되어야 하는 것이 바로 루트 모듈이다. 루트 모듈은 **@NgModule** 장식자를 이용해 정의하며 선언 형식은 아래와 같다.

{% highlight javascript %}
@NgModule({
	imports : [ 호출할 모듈1, 호출할 모듈2, ... ],
	exports : [ 노출할 모듈1, 노출할 모듈2, ... ],
	declarations : [ 사용할 구성요소1, 사용할 구성요소2, ... ],
	bootstrap : [ 애플리케이션 컴포넌트 ]
})
{% endhighlight javascript %}

**@NgModule** 장식자는 애플리케이션 모듈에 대한 메타데이터 정보를 정의한다.<br>
해당 **@NgModule** 장식자에 사용된 속성으로 **선언자(declarations), 호출자(imports), 제공자(providers), 부트스트랩(bootstrap)** 이 있다.

{% highlight javascript %}
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';

/* 어플리케이션 라우팅 모듈 */
import { AppRoutingModule } from './app-routing.module';

import { AppComponent } from './app.component';

...

@NgModule({
  imports: [
    BrowserModule,CommonModule, FormsModule,    
	 ...
	 AppRoutingModule
  ],
  providers: [],
  declarations: [
    AppComponent,
    ...
  ],
  bootstrap: [AppComponent]
  // 최상위 컴포넌트 : AppComponent
})
export class AppModule { }
{% endhighlight javascript %}

===> app.module.ts

#### imports 속성에는 Angular 라이브러리 모듈이나 라우팅 모듈을 등록한다.

대표적으로<br>

- **BrowserModule** : 브라우저 상에서 동작한다면 반드시 포함 / 컴포넌트나 지시자, 파이프 같은 구성요소를 템플릿에 나타나게 하는 역할
- **CommonModule** : 템플릿에서 사용하는 ngIf / ngFor 와 관련된 기능을 포함함 / BrowserModule 은 ommonModule 을 가지므로 BrowserModule 를 이미 선언했다면 CommonModule 를 선언 할 필요가 없다.
- **FormsModule** : 템플릿에서 자주 사용하는 NgModule 지시자나 내장 검증기 지시자등을 포함
- **RoutingModule** : 사용자가 정의할 수 있는 모듈

#### provider 에는 애플리케이션 전역에서 사용할 서비스를 등록함

#### declarations 에는 애플리케이션 레벨에서 사용하고자 하는 컴포넌트, 지시자 파이프를 선언함.

#### bootstrap 에는 최상위 컴포넌트인 애플리케이션 컴포넌트를 등록함.

{% highlight javascript %}
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: ` ...
  <div class="play-box">
    <router-outlet>< router-outlet>
  </div>`
})
export class AppComponent { }
{% endhighlight javascript %}

===> app.component.ts

app.component 를 보면 하위 특정 컴포넌트로 라우팅 후 하위 컴포넌트를 표시할 목적으로 router outlet 지시자를 포함함을 볼 수 있다. router outlet 에 표시할 컴포넌트가 있다면 애플리케이션 라우팅 모듈 설정에 다음과 같이 등록한다.

{% highlight javascript %}
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { IntroComponent } from './intro.component';
import { HelloComponent } from './hello/hello.component';

const appRoutes: Routes = [
  { path: '', component: IntroComponent },
  { path: 'hello', component: HelloComponent },
	...
];

@NgModule({
  imports: [RouterModule.forRoot(appRoutes)],
  exports: [RouterModule]
})
export class AppRoutingModule {}
{% endhighlight javascript %}

===> app-routing.module.ts

라우팅 경로 (path) 가 hello 이고 component 속성값이 HelloComponent 라면 http://localhost:4200/hello 로 접속하였을 때 HelloComponent 컴포넌트가 router outlet 에 출력된다.

이러한 설정 정보는 appRoutes 변수에 할당되며 해당 변수는 forRoot() 메서드를 이용해 등록한다.
한 가지 유의할 점은 이 메서드는 애플리케이션 라우팅 모듈에서만 한 번만 사용해야 하고, 특징 모듈 (feature module) 에서는 사용하면 안된다.

{% highlight javascript %}
import { Component } from '@angular/core';

@Component({
  selector: 'hello',
  template: `<h1>Hello!!</h1>`
})
export class HelloComponent { }
{% endhighlight javascript %}

===> hello.component.ts


**따라서 http://localhost:4200/hello 로 접속하였을 때<br> app.module.ts 애 bootstrap 에 등록된 최상위 컴포넌트 AppComponent 가 호출되고<br>해당 컴포넌트 안에 있는 router-outlet 에서는 <br>app-routing.module.ts 에 appRoute 로 등록된 HelloComponent 가 호출되어 아래와 같이**

<pre>
<h1>Hello!!</h1>
</pre>

**가 출력된다.**

