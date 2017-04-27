Intersecting two postings lists ( Continue )
============
<br>
For the intersection of two posting lists of lengths x and y, its time complexity is O(x + y)
<br>
<pre>
Intersect(p1,p2)
	answer = {}
	while p1 is not null and p2 is not null:
		if docId(p1) == docId(p2):
			add(answer,docId(p1))
			p1 <- next(p1)
			p2 <- next(p2)
		else if docId(p1) < docId(p2):
			p1 <- next(p1)
		else :
			p2 <- next(p2)
	return answer
</pre>

Completeness
------------
<br>
Prove that the algorithm INTERSECTION finds the complete list of common docIDs
<br>
Proof by mathematical induction

1. Premise<br>
Lists L1 and L2 share a set of common docIDs <d1, d2, .., dn> which are in the increasing order

2. Prove<br>
&nbsp;&nbsp;- we change the presudo code slightly that initial answer is { d0 }
&nbsp;&nbsp;- At the beginning of each Iteration where d(i-1) < docId(p1) < d(i) and d(i-1) < docId(p2) < d(i)<br>
-> answer is {d0, d1, d2, ... d(i-1)}<br>
(Loop Invariant)

3. Maintenance
<pre>
	if docId(p1) == docId(p2) == d(i):
		answer = {d0, ... , d(i)}
		d(i) < docId(p1) <= d(i+1)
		d(i) < docId(p2) <= d(i+1)
	else :
		d(i-1) < docId(p1) <= d(i)
		d(i-1) < docId(p2) <= d(i)
</pre>
&nbsp;&nbsp;=> p1 이나 p2 가 shift 되면 d(i)보다 커질수 있다?<br>
&nbsp;&nbsp;===>  아니다. Loop Invariant 에서 어긋남<br>
&nbsp;&nbsp;===> Thus, At the beginning of next Iteration, the loop invariant is still true
<br>
<br>
&nbsp;&nbsp;4. Temination<br>
&nbsp;&nbsp;For simple proof, assume docId(NULL) = d(Last number) > d(n)<br>
&nbsp;&nbsp;W.L.O.G , Let's say p1 = NULL . That is d(n) < docId(p1) <= d(L) and d(n) < docId(p2) <= d(L) <br>
&nbsp;&nbsp;By the Loop invariant, answer is {d0, ..., d(n)}