---
layout: post
title: "Ranked retrieval"
date: 2017-05-10 +0900
tags: Information_Retrieval
description: Jaccard coefficient / Bag of words model
share: true
comments: true
---

Boolean Model
----
### - Strength
- Rich expressions for queries
- Clear logical interpretation

### - Problems
- Relevancy ( = Score , **two component [ query, document ]** ) is either 1 or 0<br>many documents or few/no documents in the result<br>No term weighting in document and query is used- Difficulty for end-users for form a correct Boolean query
- **Problem with Boolean search**
	- Boolean queries often result in either too few (=0) or too many (1000s) results<br>Example)<br>Query 1: “standard user iptime N4” → 200,000 hits<br>Query 2: “standard user iptime N4 no channel found” → 0 hits
	- It takes a lot of skill to come up with a query that produces a manageable number of hits<br>( AND gives too few; OR gives too many )


--> **Solution** : **Ranked Retrieval**

Ranked retrieval
======
**Using Free Text Queries**
### - Feast or famine: not a problem in ranked retrieval

### - Query Document Matching Scores<br>( 해당 term 이 많이 있으면 High Score 부여 - Jaccard coefficient )


Jaccard coefficient
-----
![screenshot]({{ site.url }}/assets/Jaccard coefficient.png)

- A and B don't have to be the same size
- Always assings a nubmer between 0 and 1

Example)

Query : idess of march<br>
Doc1 : caesar died in march → jaccard : 1/6<br>
Doc2 : the long march → jaccard : 1/5<br>

→ 의미상으론 Doc1 이 더 가깝지만 jaccard 를 통해 Doc2 가 더 높은 rank 를 부여받는다.

- We need a more sophisticated way of normalizing for length

![screenshot]({{ site.url }}/assets/Jaccard coefficient2.png)

Bag of words model
-----
### - Term Frequency

- don't consider ordering of words
- **Term Frequency** : **tf**
- The term frequency tf(t,d) of term t in document d is defined as the number of times that t occurs in d.

#### Log-frequency weighting

![screenshot]({{ site.url }}/assets/Log-frequency weighting.png)

Example)<br>
Doc1 : Hanyang Ansan Univ Hanyang<br>
Query : Hanyang Ansan<br>
→ score(query,doc1) = (1 + log(2)) : Hanyang 2번  + (1 + log(0)) : Ansan 0번 = 2

### - Document frequency

- Rare terms are more informative than frequent terms<br>
	→ We want a high weight for rare terms like arachnocentric.8
	
- **Document frequency** : **df**

#### idf weight

![screenshot]({{ site.url }}/assets/idf weight.png)


#### Effect of idf on ranking

- idf has no effect on ranking one term queries<br>( if one term, same ranking with df )
- idf affects the ranking of documents for queries with at least two terms


### tf-idf weighting

![screenshot]({{ site.url }}/assets/tf-idf weighting.png)

#### Score for a document given a query

![screenshot]({{ site.url }}/assets/Score for a document given a query.png)

Example)<br>
Query : Hanyang Univ<br>
Doc1 : Hanyang Ansan Univ Hanyang<br>
Doc2 : Hanyang Ansan<br>
→ score(query, doc1) = [ ( 1 + log(2) ) x log (10/2) ] : Hanyang + [ 1 x log(10/1) ] : Univ<br>
→ score(query, doc2) = [ (1) x log(10/1) ] : Hanyang<br>

	
