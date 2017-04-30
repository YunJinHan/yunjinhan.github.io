---
layout: post
title: "Angular2 Module"
date: 2017-05-01 01:00 +0900
tags: Angular2
description: Angular2 Module - 모듈이란? / 모듈 종류
share: true
comments: true
---

Angular2 Module
=====
Angular 는 타입스크립트 모듈의 구성을 관리할 수 있게 모듈 시스템을 제공한다.<br>
Angular 는 여러 모듈로 구성된 하나의 모듈 집합으로 볼 수 있다.

### 주요 Angular 라이브러리 모듈
- **@angular/common** : 파이프, 구조지시자, 속성지시자와 관련된 모듈 포함
- **@angular/core** : Angular 를 구성하는 주요 요소의 장식자 및 핵심 모듈을 포함
- **@angular/forms** : 폼 관련 모듈이나 지시자를 포함
- **@angular/http** : HTTP 통신과 관련된 모듈을 포함
- **@angular/platfrom-browser** : 브라우져 모듈, DOM sanitizer 등 브라우저와 관련된 모듈 포함
- **@angular/router** : 라우터 관련 모듈이나 지시자 포함
- **@angular/testing** : 테스팅 관련 모듈 포함

{% highlight javascript %}
export class Message {
	sayHello() {
		return "Hello";
	}
}
export var DISTRICT : District[] = [
	{ id : 1, name : '서울' }, { id : 2, name : '부산'}
];
export function echo(msg : string) {
	return msg;
}
{% endhighlight javascript %}

===> hello.module.ts

#### 외부로 공개할 모듈은 export 를 이용하여 선언한다.<br>export 키워드로 선언하면 다른 모듈에서 해당 모듈로 접근 가능한 상태가 된다.

{% highlight javascript %}
import { Message, DISTRICT, echo } from './hello.module';
{% endhighlight javascript %}

#### export 키워드로 공개 설정된 모듈을 모두 사용하려면 Import 키워드를 이용해 사용한다.


#### < Angular 모듈 종류 >
1. **루트 모듈** : 최상위 모듈을 통해 애플리케이션 모듈을 구성
2. **특징 모듈** : 단위 애플리케이션을 구성하는 모듈
3. **공유 모둘** : 다르 모듈에 포함되어 동작하는 모듈
4. **핵심 모듈** : 항상 동작할 필요가 있거나 애플리케이션 전체적인 동작에 핵심적인 역할을 하는 모듈