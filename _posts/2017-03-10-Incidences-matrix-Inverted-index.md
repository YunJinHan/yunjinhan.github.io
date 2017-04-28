---
layout: post
title: "Term Document incidences matrix / Inverted index"
date: 2017-03-10 +0900
tags: Information_Retrieval
description: Term Document incidences matrix / Inverted index
share: true
comments: true
---

1.Term Document incidences matrix
=============

Using incidence vectors and Boolean model ( bitwise operation )
-------------

ex) Search Word = t[i]

	t[1] 과 t[2] 를 포함하고 t[3] 를 포함하지 않는 document 를 찾아라.
	
	=>		docu 1		docu 2		docu 3
	t[1]	  1			  1		       0
	t[2]	  1			  0			   1
	t[3]	  0			  1			   0

	-> t[1] 포함 = 110
	   t[2] 포함 = 101
	   t[3] 미포함 = 101 (010 complements)
	   & (AND) operation result = 100


Incidences matrix problem
-------------

1.&nbsp;&nbsp;Document 가 많아지면 해당 필요 Bit 가 많아진다. ( *Bigger Collections* )

   ex) 1,000,000 Document 있을시 10,000,000 bit 필요

2.&nbsp;&nbsp;해당 boolean model 에서 bit 의 낭비가 심하다.

   ex) 보통 1 M bit 중 1000 개의 bit 만이 1의 값을 가짐 ( 0가 많아 공간낭비 )




2.Inverted index
=============

Search Word = t[i] , t[i] 를 포함하는 Document 의 DocId 를 list 에 저장함.

( Identify each document by docId / Size 를 줄이기 위해 document url 대신 docId 사용함. )

How to convert DocId to Url ? => Hashing or B-Tree

ex) Search Word = t[i]
	
	t[1] = { 1, 2, 4, ... }
	t[2] = { 1, 5, 6, ... }
	t[3] = { 7, 13, 15, ... }
	
	t[i] => Dictionary
	{ ... } => List ( set of datastructure and has variable length of each list , sort by DocId )

	=> 1, 2, 4, ... -> DocId

	each one is *Posting*. ( Posting = { DocId, Position of list } )


Inverted index problem
------------

1.&nbsp;&nbsp;List 중간에 DocId 삽입시 overhead 증가 -> B+ Tree 사용



