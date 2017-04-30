---
layout: post
title: "Inverted Index"
date: 2017-03-13 +0900
tags: Information_Retrieval
description: Inverted index
share: true
comments: true
---

Inverted Index
=============

Inverted Index Construction
-------------
{% highlight javascript %}
			Document			ex) Friends are Romans
				▼
			Tokenizer			Friends, are, Romans
				▼
			Linguistic Modules		friend, are, roman
				▼
			Indexer				friend -> posting = (2, 4, ...)
							roman -> posting = (13, 16 ...)
{% endhighlight javascript %}

Initial Stages of Text Processing
-------------

### Normalization
##### Map text and query term to same form
ex) you want U.S.A and USA to match

### Tokenization
##### Cut Character sequence into word tokens

### Stemming (어근)
##### we may wish different forms of a root to match
ex) computer, computing, computation .. => compute

### Stop words
##### we may omit very common words (or not)
ex) the, a, of, to ...


Indexer step 
-------------

### Step 1) Token sequence
####		 sequence of ( Modified token, docID ) paris

### Step 2) Sorts
####		 sort by terms and docID

### Step 3) Dictionary & Postings
#### - 1. Multiple term entries in a single document are merged
#### - 2. Split into dictionary and posting
#### - 3. Document frequency information is added
=> Has O(nlogn) time -> using Map Reduce to reduce time

ex) 

Doc1 boy deep fire sea<br>
Doc2 nation sea deep<br>
Doc3 nation apple fire ban<br>
Doc4 nine nation boy people<br>

	step 1. (boy, doc1) (deep, doc1) (fire, doc1) (sea, doc1)
			(nation, doc2) (sea, doc2) (deep, doc2)
			(nation, doc3) (apple, doc3) (fire, doc3) (ban, doc3)
			(nine, doc4) (nation, doc4) (boy, doc4) (people, doc4)

	step 2. (apple, doc3)
			(ban, doc3)
			(boy, doc1)
			(boy, doc4)
			(deep, doc1)
			(deep, doc2)
			(fire, doc1)
			(fire, doc3)
			(nation, doc2)
			(nation, doc3)
			(nation, doc4)
			(nine, doc4)
			(sea, doc1)
			(sea, doc2)
			(people, doc4)

	step 3. apple : 1 -> doc3
			ban : 1 -> doc3
			boy : 2 -> doc1, doc4
			deep : 2 -> doc1, doc2
			fire : 2 -> doc1, doc3
			nation : 3 -> doc2, doc3, doc4
			nine : 1 -> doc4
			sea : 2 -> doc1, doc2
			people : 1 -> doc4



Query Processing
-------------
#### Step 1). AND operation
(consider processing the query A AND B)
#### Step 2). Locate A in Dictionary
#### Step 3). Locate B in Dictionary
#### Step 4). Merge the two posting and find interested things
- 'A' token Dictionary / 'B' token Dictionary -> insert hash table == O(n) time<br>
But! hash table 크기가 현실적으로 매우커서 메모리에 상주하게 두고 검색 불가능

- pointer X ▼<br>
  'A' :  word1 / word2 ...<br>
  pointer Y ▼<br>
  'B' :  word2 / word8 ...<br><br>
  Two pointer x, y => 해당 포인터 값이 더 작은게 한칸씩 이동하며 비교<br>
  포인터 값끼리 비교하며 같은 값이 있는 경우 뽑아냄<br><br>
  if A length is A' and B length is B' , O(A'+B') linear time<br>
  **Curial : Each posting must be sorted by DocId**



