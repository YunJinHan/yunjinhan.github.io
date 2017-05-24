---
layout: post
title: "Unranked Retrieval Evaluation"
date: 2017-05-24 +0900
tags: Information_Retrieval
description: Precision and Recall
comments: true
---

Unranked Retrieval Evaluation
=====

Precision and Recall
-----
### Precision ( 정확도 ) : Fraction of retrieved docs that are relevant<br> P = relevant / retrieved


Relevant : 검색결과에 포함된 것
Retrieved : 검색결과와 관련된 것

### Recall : Fraction of relevant docs that are retrieved<br> P = retrieved / relevant

![screenshot]({{ site.url }}/assets/precision_recall.png)

tp : ture positive / fp : false positive<br>
fn : false negative / tn : ture negative<br><br>
ture : is relevant / false : is not relevant<br>
positive : include docs / negative : do not include docs


Precision@K
-----
- Set a rank threshold K
- Compute % relevant in top K
- Ignores docs ranked lower than K
- Example )
	- ![screenshot]({{ site.url }}/assets/precision_ex.png)
	- Prec@3 : 2/3 ( top 3개의 docs 중 몇 개가 relevant 한 docs 인가 )
	- Prec@4 : 2/4
	- Prec@5 : 3/5

Recall@K
-----
- Example )
	- ![screenshot]({{ site.url }}/assets/precision_ex.png)
	- R (relevant docs) = 3 개
	- R@3 = 2/3 (relevant docs 중 몇 개가 relevant 한 docs 인가)
	- R@4 = 2/3

	
Precision - Recall Curve
-----
![screenshot]({{ site.url }}/assets/precision_recall_curve.png)

검색 엔진이 좋은 경우 : 빨간 선
검색 엔진이 나쁜 경우 : 노란 선


Mean Average Precision
-----
- Consider rank position of each **Relevant** docs
- Compute Precision@K for each K1, K2, ... Kr
- **Average Precision = average of P@K**

	**K = 3**

![screenshot]({{ site.url }}/assets/MAP.png)

- MAP is Average Precision across multiple queries / rankings


### Average Precision


![screenshot]({{ site.url }}/assets/average precision.png)

<pre>
< Ranking #1 >
# of docs = 10

--> Mean Average Precision = ( 0.78 + 0.52 ) / 2 = 0.65
</pre>

**보통 Relevant 의 Precision 만으로 평균을 낸다.**