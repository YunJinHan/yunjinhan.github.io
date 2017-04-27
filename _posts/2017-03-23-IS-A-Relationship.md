---
layout: post
title: "IS A relationship"
date: 2017-03-23 +0900
tags: Object-Oriented-Programming
description: Inheritance models an IS A relationship
---

Inheritance models an IS A relationship (aka Generalization / Specialization realtionship) among the classes
---------------

== > Public Inheritance relationship is **IS A** relationship<br>
( 부모 인터페이스를 자식 인터페이스의 일부로 포함시킴 )

![screenshot]({{ site.url }}/assets/dependency.jpg)

자식이 부모에 dependent 하다. (**data flow 가 아닌 dependency 의존관계 표현**)

A class can be a superclass as well as a subclass at the same time and the IS A  relationship is transtive
---------------
Subclasses inherit all sttributes and operations from its superclass and its ancestors
---------------

Subclasses can be redefine inherited operations (**Overriding**)
---------------
-->&nbsp;&nbsp;Operations must have the same **Signatures**<br>
-->&nbsp;&nbsp;Subclasses can alter or override information inherited from parent classes
<br>
<br>

Anything that is true of an object of Person type is also true of an object of Student type, but Not vice - versa &nbsp;&nbsp;(부모에 성립되는 것들은 자식에서도 성립된다.)
----------------
Anything a superclass can do, a subclass can do as well
<br>
Subtyping uses inheritance as a mechanism for **Interface sharing**
----------------
- enforces the **IS A** relationship
- also called **Interface inheritance**

Subclassing uses inheritance as a mechanism for Implementation sharing
-----------------
- enforces implemented in terms of relationship
- also called class inheritance or implementation inheritance<br>-> private Inheritance ( only C++ )

Pure virtual function
-------------------
- to have derived class inherit a function interface only

Virtual function
---------------
- to have derived classes inherit a function interface as well as a default implementation<br> (**overriding O == abstract method**)

Nonvirtual function
-----------------
- to have derived classes inherit a function interfaces as well as a mandatory implementation<br>(**overriding X == final method**)


