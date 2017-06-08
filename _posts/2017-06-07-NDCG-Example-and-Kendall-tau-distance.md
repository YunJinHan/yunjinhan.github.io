---
layout: post
title: "NDCG Exercise and Kendall tau distance"
date: 2017-06-07 +0900
tags: Information_Retrieval
description: Kendall tau distance
comments: true
---

NDCG Exercise
----
![test]({{ site.url }}/assets/NDCG_Exercise.png)

1. System1 MAP : (1/1 + 2/3 + 3/9 + 4/10) * 1/4
2. System2 MAP : (1/2 + 2/5 + 3/6 + 4/7) * 1/4<br>**System1 has higher MAP than System2**
3. System1 DCG : 1 + 1/log3 + 1/log9 + 1/log10
4. System2 DCG : 1 + 1/log5 + 1/log6 + 1/log7
5. MaxDCG ( R 을 전부 앞으로 옮긴 후 계산 ) : 1 + 1/log2 + 1/log3 + 1/log4
6. NDCG = DCG / MaxDCG

Kendall Tau Distance
----
X is agree key / Y is disagree key

- **X + Y is size of benchmark data**

--> Kendall tau distance : (X-Y) / (X+Y)

1 is **Maximum** value and -1 is **Minimun** value

Example)
<pre>
p = { (1,2) , (1,3) , (1,4) , (2,3) , (2,4) , (3,4) }
(1,2) means 1 is better than 2
A = ( 1 , 3 , 2 , 4 )
==> X = 5 , Y = 1 ( p 에서 맞는 것 5개 틀린 것 1개 )
Kendall tau distance : (X-Y) / (X+Y) = 4 / 6 = 2/3
</pre>

Kendall Tau Distance Exercise
----
![test]({{ site.url }}/assets/kendall_Exercise.png)