---
layout: post
title: "B+ Tree (cont'd)"
date: 2017-03-27 +0900
tags: Information_Retrieval
description: B+ Tree (cont'd)
---

Insertion in B+ Trees
------------
### - Leaf Node 에 New Key 값이 Insert 될 자리가 있을때
해당 New Key 값을 Order 에 맞게 자리를 찾아 Insert 한다.

### - Leaf Node 에 New Key 값이 Insert 될 자리가 없을때
**해당 Node 의 values 가 꽉차서 Overflow 가 발생하게 된다** ==> **Split 해준다.**<br>

Example - Insert 8<br>

![screenshot]({{ site.url }}/assets/b+_tree_insertion.png)

- Splitting a Leaf Node
	1. 8 value 를 Insert 시 해당 Leaf Node 의 values 들이 2 3 5 7 8 이 되는데<br>4 개를 초과하여 **Overflow** 가 발생하게 된다.
	2. 기존의 Node A 에 P(1),K(1) ~ P( |n/2| ),K( |n/2| ) , P( |n/2| + 1 ) 까지 Copy 한다.<br>( |A| = A 보다 큰 정수들 중 가장 작은 정수 )<br>( n = 5, |n/2| = 3 , 결과 : [ 2, 3, 5 ])
	3. 새로운 Node B 를 만들어 P( |n/2| ),K ( |n/2| ) ~ P( n ), K( n ) , P( n + 1 ) 까지 Copy 한다.<br>( 결과 : [ 7, 8 ] )
	4. 부모 노드에 K( |n/2| + 1 ) 값을 올린다. 왼쪽 자식 포인터는 A Node, 오른쪽 자식 포인터는 B Node 를 가르킨다.


- Splitting a Non Leaf Node ( Internal Node ) 

	1. 7 value 가 올라와서 해당 Internal Node ( 여기선 Root ) 의 values 들이 7 13 17 24 30 이 되는데 <br>4 개를 초과하여 **Overflow** 가 발생하게 된다.
	2. 올라온 값을 Insert 한다.
	3. 기존의 Node A 에 P(1),K(1) ~ P( |n/2| - 1 ),K( |n/2| - 1 ) , P( |n/2| ) 까지 Copy 한다.<br>( |A| = A 보다 큰 정수들 중 가장 작은 정수 )<br>( n = 5 ( 7, 13, 17, 24, 30 ), |n/2| = 3 ===> 결과 : [ 7, 13 ] )
	4. 새로운 Node B 를 만들어 P( |n/2| + 1 ),K( |n/2| + 1 ) ~ P(n),K(n) , P( n + 1 ) 를 Copy 한다.<br>( 결과 : [ 24, 30 ] )
	5. K( |n/2| ) 값을 새로운 부모노드에 Insert 한 후 왼쪽 자식 포인터는 A Node, 오른쪽 자식 포인터는 B Node 를 가르킨다.

	==> 가운데 값을 부모 노드로 올리고 양쪽의 값을 나누어 2개의 노드로 만든다.

Deletion in B+ Trees
------------
### - 삭제 후 Leaf Node 의 Condition 이 유지되는 경우
해당 Key 값을 제거 한 후 해당 삭제가 발생된 Node 안에서 한칸씩 당겨 정렬한다.
### - 삭제 후 Leaf Node 의 Condition 이 유지되지 않는 경우

![screenshot]({{ site.url }}/assets/b+_tree_deletion1.png)

- Re-distribution in Leaf Nodes
	
	1. 삭제 시 **Underflow** 가 발생하게 된다.
	2. 형제 Node 에서 Key 값을 빌려온다. ( 왼쪽 형제이면 가장 큰 Key 를 , 오른쪽 형제이면 가장 작은 Key 를 빌려온다. )
	3. 해당 삭제가 발생한 Node 에 빌려온 Key 값을 넣고 **빌려준 Node 에서는 가장 작거나 큰 값을 부모 노드로 밀어 올린다. ( Copy Up )**<br>( ex. 18 을 빌려와 부모에서 18이 내려오고 19가 부모로 올라간다 --> 3단 밀어내기 )

	<br>

![screenshot]({{ site.url }}/assets/b+_tree_deletion2.png)

- Merge in Leaf Nodes and Re-distribution in Non-leaf Nodes

	1. 삭제를 하였는데 형제 Node 에서 빌려올 Key 값이 없다.
	2. 해당 형제 노드들을 **Merge** 한다.
	3. 부모에 Key 값이 한 개 있는 경우 ( Leaf Node 의 Key 값을 제외한 Key 값의 수 )
		- 부모 또한 **Merge** 를 시도한다. 실패 한다면 형제 부모 Node 에서 Key 값을 빌려온다.
	4. 부모에 Key 값이 아무것도 없는 경우 ( Leaf Node 의 Key 값을 제외한 Key 값의 수 )
		- 부모가 형제 부모 Node 에서 Key 값을 빌려온다. 실패 한다면 부모 또한 **Merge** 를 시도한다.

<br>

Example
--------------
### - Delete 24
1.
![screenshot]({{ site.url }}/assets/b+_tree_deletion3.png)


2.
![screenshot]({{ site.url }}/assets/b+_tree_deletion3_1.png)


try that N = 3 && insert 6, 8, 5, 7, 4 && delete 8 || delete 6 !!

