---
layout: post
title: "Blocking NonBlocking Synchronous Asychronous"
date: 2017-06-16 +0900
tags: NodeJS
description: Blocking NonBlocking Synchronous Asychronous
share: true
comments: true
---

Blocking / NonBlocking
======

@ Blocking
----
- Waiting for System call's completion
- Return with data ( 실행된 결과 )
- Waiting in a wating Queue

@ NonBlocking
----
- Immediate return
- Return with data ( 실행된 결과 )


**블러킹과 논블러킹의 차이점은 블러킹은 대기큐에 들어가고 시스템 콜이 완료되길 기다리며 해당 기다리는 동안은 어떤일도 하지 못한다는 점이고, 논블러킹은 대기큐에 들어가지 않으며 시스템 콜이 완료되길 기다리지 않고 빠르게 다음일을 수행한다는 점이다.**


Synchronous / Asynchronous
=====

@ Sysnchronous
----
- Waiting for System call's completion
- Return with data ( 실행된 결과 )

@ Asynchronous
----
- Immediate return


**동기와 비동기의 차이점은 블러킹과 논블러킹과 유사하게 동기는 시스템 콜이 완료되길 기다리며 해당 기다리는 동안은 어떤일도 하지 못하며 시스템 콜이 리턴될때 데이터와 함께 리턴되고, 비동기는 기다리지 않고 다음일을 바로 수행 후 데이터가 함께오지 않는 리턴만을 인지한다는 점이다**

<br><br><br>

Blocking VS Synchronous
----
- **시스템 콜의 리턴을 기다리는 동안 대기큐에 머무르는 것이 필수이면 Blocking 이며 필수가 아니면 Synchronous 이다**

NonBlocking VS Asynchronous
----
- **시스템 콜이 리턴될때 데이터와 함께 리턴된다면 NonBlocking 이며 그냥 완료 리턴만 온다면 Asynchronous 이다**

