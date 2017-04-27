---
layout: post
title: "Angular2 Component"
date: 2017-04-13 +0900
tags: Angular2
description: Angular2 Component
---


Angular Component
===========
Angular Component 는 **Web Component 기술**을 기반으로 구성되며 <br>기존 블록 엘리먼트 구조를 **Component 구조**로 전환시켜 개발한다.

#### - 중첩 Component
자식 Component 와 부모 Component 를 사용<br>부모 Component 가 여러 자식 Component 를 포함할 때 부모 Component = 중첩 Component
#### - Component Tree
Component 끼리의 관계를 나타내는 것으로 Component 사이의 의존성이 존재할 수 있다.
#### - Component 기반 개발
<pre>
@Component({
	// 컴포넌트 메타 데이터 설정
})
export class HelloComponent {
	// 컴포넌트 로직 작성
}
</pre>
< Web Component 기술 요소 >
-----
#### - HTML 템플릿
**Angular 의 템플릿은 Web Component 기술의 템플릿 기술을 이용한다.**
<pre>
< template id="nav-item-template">
	< div class="nav">
		< div class="item">메인</div>
		< div class="item">서비스 소개</div>
		< div class="item">서비스 특징</div>
	< /div>
< /template>
</pre>
#### - 템플릿 호출
**Angular 에서 템플릿 호출 기능은 Web Component 호출 기술을 이용한다.**
<pre>
< head>
< link rel="import" href="template.html">
< /head>
...
< script>
	var link = document.quertSelector('link[re]="import"]');
	var content = link.import;
	var el = content.querySelector('#template');
	document.body.appendChild(el.cloneNode(true));
< /script>
</pre>
#### - 쉐도우 DOM
**Angular Component 는 쉐도우 DOM 을 사용하여 문서 트리에 영향을 받지 않는 쉐도우 DOM 을 이용한다.**<br><br>
DOM 은 크게 문서 DOM 과 쉐도우 DOM 으로 나누어진다.<br>
문서 DOM 은 현재 페이지에 대한 DOM 이며, 쉐도우 DOM 은 웹 페이지가 실행되는 중 생성되는 가상 DOM 이다.
#### - 커스텀 엘리먼트
**Angular Component 의 엘리먼트 이름은 Web Component 의 커스텀 엘리먼트 기술을  이용한다.**
<pre>
< hello-button>< /hello-button>
</pre>
<br>
&nbsp;

Angular Component 구조
=======
### - Import 영역
<pre>
import { Component } from '@angular/core'; 
</pre>
Angular 라이브러리 모듈을 호출하거나 사용자 모듈을 호출할때 Import 키워드를 사용한다.<br>
Angular 라이브러리 모듈은 **@** 를 붙이며 사용자 모듈은 **./** 을 통해 외부 모듈을 호출한다.

### - Component 장식자 영역
<pre>
@Component({
	selector : 'intro -component',
	template : '< div>App Hello< /div>',
	styles : ['div{ background: blue; }']
})
export class AppHello { }
</pre>
@Component 는 Component 장식자라고 불리며 위와 같은 형식을 가진다.

- selector 속성<br>Component 의 이름을 정의. < intro -component>< /intro -component> 같은 형식으로 정의<br>
- template 속성<br>Component 에서 가장 중요한 속성이며 UI 코드를 정의한다.<br>**template** : 내부 파일에 HTML 과 템플릿 문법을 이용해 템플릿을 정의한다.<br>**templateUrl** : 외부 파일에 HTML 과 템플릿 문법을 이용해 템플릿을 정의한다.
- style 속성<br>템플릿에 스타일을 추가하기위해 CSS 스타일을 설정한다.<br>**styles** : 템플릿에 대한 스타일을 현재 파일 내부에 정의한다.<br>**styleUrl** : 템플릿에 대한 스타일을 외부 CSS 파일에 정의한다.


### - Component Class 영역
Component Class 에서는 템플릿 데이터 출력과 관련된 로직을 처리한다.

- 외부에 정의된 HTTP 서비스를 이용해 HTTP 요청 결과를 받아 템플릿에 데이터를 반영하는 로직
- 템플릿으로부터 클릭 이벤트를 받아 클릭 이벤트에 대한 처리를 수행하는 로직
- 템플릿에 사용할 데이터를 다른 Component 로 부터 전달 받아 이를 처리하는 로직
- 바인딩 변수를 이용해 권한에 따라 템플릿에서 화면 제어를 담당하는 로직


