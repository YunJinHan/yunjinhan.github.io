---
layout: post
title: "N - Gram Overlap"
date: 2017-04-16 +0900
tags: Information_Retrieval
description: N - Gram Overlap
share: true
comments: true
---

N - Gram Overlap
=====
Use the n-gram index (recall wild-card search) to **retrieve all lexicon terms matching any of the query n-grams**

Example )
{% highlight javascript %}
Trigrams
* november -> nov, ove, vem, emb, mbe, ber
* december -> dec, ece, cem, emb, mbe, ber
<br>--> emb, mbe, ber 3 trigrams overlap ( of 6 in each term )
{% endhighlight javascript %}

### - Simple Lower Bound of Substring Edit Distance
**정확하진 않지만 Lower Bound ( 근사값 ) - The minimun number of edit operations**

![screenshot]({{ site.url }}/assets/SimpleLowerBound.png)

{% highlight javascript %}
Trigrams --> n == 3

∂ = Jackson Pollock ( len = 15 )
s = JalksenPollock

Jac, ack, cks, kso, son, on_, n_P, _Po
Jal, alk, lks, kse, sen, enP, nPo, Pol&nbsp;&nbsp;===>&nbsp;8 mismatching trigrams

Pol, oll, llo, loc, ock ===> 5 matching trigrams ( = c )

max(|s|,|∂|) - n + 1 - c = max(14,15) - 3 + 1 - 5 = 8

So, Simple Lower Bound of Substring Edit Distance is [8/3] = 3
{% endhighlight javascript %}
<br>


SOUNDEX
======
**Class of heuristics to expand a query into phonetic equivalents**<br>
발음이 동등한지 여부를 판단한다

1. Retain the first letter of the word ( **첫 글자는 생략** )

2. Change all occurrences of the following letters to '0' (zero)
	- A, E, I, O, U, H, W, Y
3. Change letters to digits as follows
	- B, F, P, V : 1
	- C,G,J,K,Q,S,X,Z : 2
	- D,T : 3
	- L : 4
	- M, N : 5
	- R : 6
--> 외울 필요 없음

4. Repeatedly remove one out of each pair of consecutive identical digits.( **반복되는 Pair 제거** )
5. Remove all zeros from the resulting string ( **0 모두 지움** )
6. Pad the resulting string with trailing zeros and return the first four positions ( **길이 맞추기 위해 0 padding** )

<br>
Example )

![screenshot]({{ site.url }}/assets/soundex.png)

What queries can we process?
------
- Positional inverted index with skip pointers
- Wild-card index
- Spell-correction
- Soundex

Query Search Engine Such as (SPELL(moriset) /3 toron*to) OR
SOUNDEX(chaikofski) ...