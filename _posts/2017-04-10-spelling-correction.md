---
layout: post
title: "Spelling Correction"
date: 2017-04-10 +0900
tags: Information_Retrieval
description: Spelling Correction
share: true
comments: true
---

Spelling Correction
=======

### - Isolated Word
- Check each word on its own for misspelling
- Will not catch typos resulting in correctly spelled words<br>( from -> form 둘 다 존재하는 단어이기 때문에 무엇이 맞는지 모른다. )
- So cannot find correctly spelled words

### - Context Sensitive
- Look at surrounding words
- Complexity

Isolated Word Correction
------
- Fundamental premise - there is a lexicon from which the correct speelings come
- Two basic choices for this
	- **A Standard Lexicon**<br>( 사전에 있는 단어인지 체크 후 없다면 비슷한 단어로 고침 )
	- **The Lexicon of the indexed corpus**

#### - 3 kinds of several alternatives
- **Edit Distance (Levenshtein Distance)**
- **Weighted Edit Distance**
- **N-gram overlap**

< Edit Distance >
-------
- Give two strings S1 and S2, the minimum number of operations to convert one to the other
- Operations are typically character-level<br>**Insert, Delete, Replace**

<pre>
				Edit Distance
dof , dog ==>		  1
cat , act ==>		  2
cat , dog ==>		  3
</pre>

- Generally found by **Dynamic Programming**

### - Levenshtein
- Edit Distance Metrics
	- Distance is shortest sequence of edit commands that transform s to t.
- Simplest set of operations
	- Copy char from s over to t
	- Delete char in s ( cost 1 )
	- Insert char in t ( cost 1 )

![screenshot]({{ site.url }}/assets/Levenshtein.png)

#### - Dynamic Algorithm

![screenshot]({{ site.url }}/assets/Levenshtein2.png)

==> **d(s(i),t(j))** : s(i) 와  t(j) 를  비교하여 같다면 **0** 다르다면  **1** 를 가진다.

![screenshot]({{ site.url }}/assets/editDistanceAlgorithm.png)

==> **this algorithm has O(m-n) time Complexity**<br>
==> **c(s(i),t(j))** : a(i) 와  b(j) 를  비교하여 같다면 **0** 다르다면  **1** 를 가진다.

![screenshot]({{ site.url }}/assets/editDistanceEx.png)