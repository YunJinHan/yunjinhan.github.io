---
layout: post
title: "Angular Component2"
date: 2017-04-29 +0900
tags: Angular2
description: Angular Component - Input / Output / EvenEmitter
share: true
comments: true
---

Angular Component2
=====
Input 을 이용한 값 전달
-----
### - input 장식자를 이용한 값 받기
@input 장식자는 외부에서 전달된 값을 받기 위해 사용된다.<br>( 부모에서 자식으로 값을 전달할 때 많이 사용됨 )

<pre>
import { Component } from '@angular/core';
@Component({
	selector: 'parent-to-child-input',
	template: `< div>부모
	< child-input [name1]="fruit1" [name2]="fruit2()" [name3]="fruit3"
	  [name4]="1+1" [name5]="fruit5" [name6]="fruit6">< /child-input>
	< /div>`,
	styles: [`div{border : 2px soild #666; width:90%; height:50%}`]
})

export class ParentToChildInputComponent {
	fruit1 = "사과";
	fruit2() {
		return "배";
	}
	fruit3 = ['딸기','포도','참외'];
	static fruit5 = "수박";
	get fruit6() {
		return ParentToChildInputComponent.fruit5;
	}
}
</pre>
==> 부모 컴포넌트 ( 자식에게 값을 전달함 )

***정적변수 static ( fruit5 ) 은 자식 컴포넌트로 곧바로 전달할 수 없다.<br>
곧바로 전달하려면 getter 메소드 ( fruit6 ) 를 사용해야 한다***

<pre>
import { Component, Input } from '@angular/core';
@Component({
	selector: 'child-input',
	template: `< div>
	자식< br>
	name1 : {{name1}}< br>
	name2 : {{name2}}< br>
	name3 : < span *ngFor="let i of name3">{{i}}< /span>< br>
	name4 : {{name4}}< br>
	name5 : {{name5}}< br>
	name6 : {{name6}}< br>
	< /div>`,
	styles: [`div{border : 2px soild #666; width:90%; height:50%}`]
})

export class ChildInputComponent {
	@Input() name1 : string;
	@Input() name2 : string;
	@Input() name3 : string[];
	@Input() name4 : number;
	@Input() name5 : string;
	@Input() name6 : string;
}
</pre>
===> 자식 컴포넌트 ( 부모로부터 값을 전달받음 )<br>
<br>
< 출력 결과 >
<pre>
부모
자식
name1 : 사과
name2 : 배
name3 : 딸기 포도 참외
name4 : 2
name5 :
name6 : 수박
</pre>


### - input 속성을 이용한 값 받기
Component 장식자 설정의 inputs 속성을 통하여 값을 주고 받는 것이 가능하다.

<pre>
import { Component } from '@angular/core';
@Component({
	selector: 'app-parent-to-child-inputs',
	template: `< div>부모
	< child-inputs [name1]="name1" [name2]="name2">
	< /child-input>< /div>`,
	styles: [`div{border : 2px soild #666; width:90%; height:50%}`]
})

export class ParentToChildInputsComponent {
	name1 = "사과";
	name2 = "바나나";
}
</pre>
===> 부모 컴포넌트

<pre>
import { Component, Input } from '@angular/core';
@Component({
	selector: 'child-inputs',
	template: `< div>자식< br>
	input 값 : {{name1}}, {{name2}} < /div>`,
	styles: [`div{border : 2px soild #666; width:90%; height:50%}`],
	inputs: ['name1','name2']
})

export class ChildInputsComponent {
	name1 : string;
	name2 : string;
}
</pre>
===> 자식 컴포넌트

EventEmitter 를 이용한 값 전달
-----
***부모 컴포넌트로 값을 전달하려면 @Output 장식자로 선언한 변수를 EvenEmitter 로 초기화 한다.<br>
그리고 부모에게 보낼 시점이 되면 emit() 메서드를 통하여 부모로 이벤트를 전달한다.***

<pre>
import { Component, EventEmitter, Output } from '@angular/core';
@Component({
	selector: 'child',
	template: `< div>자식
	< button (click)="updateParent(active)">부모에게 이벤트보내기
	< /button>< /div>`,
	styles: [`div{border : 2px soild #666; width:90%; height:50%}`]
})

export class ChildComponent {
	active = false;
	@Output() outputProperty = new EventEmitter<boolean>();
	
	updateParent(active: boolean) {
		this.active = !active;
		this.outputProperty.emit(this.active);
	}
}
</pre>
===> 자식 컴포넌트<br>
***EventEmiiter 객체의 자료형은 boolean 으로 선언되어있으며 받는 측에서도 동일한 자료형으로 받아야 하며 이벤트가 발생하면 emit() 을 통해 전달된다.***

<pre>
import { Component } from '@angular/core';
@Component({
	selector: 'app-root',
	template: `< div>부모
				클릭수 : {{cntClick}}< br>
				자식상태 : {{active}}< br>
				< child (outputProperty)="outputEvent($event)">
				< /child>< /div>`,
	styles: [`div{border : 2px soild #666; width:90%; height:50%}`]
})

export class ChildToParentComponent {
	cntClick = 0;
	active = false;
	
	outputProperty(active: boolean) {
		this.cntClick ++;
		this.active = active; ( 자식으로부터 받은 active 값 )
	}
}
</pre>
===> 부모 컴포넌트