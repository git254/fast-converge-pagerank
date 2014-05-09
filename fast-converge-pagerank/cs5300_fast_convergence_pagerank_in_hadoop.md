# CS5300 Fast Convergence PageRank in Hadoop

![Mou icon](https://raw.githubusercontent.com/git254/image/master/aws-hadoop.png)
![](https://raw.githubusercontent.com/git254/image/master/PR.png)

## Overview

Implementing **blocked pagerank** to achieve better convergence. </br>
<http://edu-cornell-cs-cs5300s14-project2.s3.amazonaws.com/project2.html>

## Map Reduce Illustration
![Mou icon](http://img.my.csdn.net/uploads/201204/10/1334068675_4451.png)

### Naive Page Rank 
	class MAPPER
		method MAP(nid n, node N)
			p <-- N.PageRank/|N.AdjacencyList|
			EMIT(nid n, N)    
			for all nodeid m from N.AdjacencyList do
				EMIT(nid m, p)

	class REDUCER
		method REDUCE(nid m, [p1,p2,...])
			M <-- NONE
			for all p from counts[p1,p2,...] do
				if IsNode(p) then
					M <-- p
				else
					s <-- s + p
			M.PAGERANK <-- s
			EMIT(nid m, node M)



#### Naive PR Input Format
'nodeid PageRank Neighbour-List'
#### Naive PR Output Format
'nodeid PageRank'

#### Naive PR Code Description
###### PageRank.java
Driven class to create Map Reduce Job and configure input/output path
###### Mapper.java
Implements Mapper class in above seudo code
###### Reduce.ava
Implements Reducer class in above seudo code
###### LeftoverMapper.java (optional)
Doing nothing, just pass data to LeftoverReducer
###### LeftoverReducer.java (optional)
Re-distributes Leftover mass

#### Naive PR Converge 
PR not converge even after 12 passes

### Blocked Page Rank
Each Reduce task loads its entire Block into memory and does multiple in-memory PageRank iterations on the Block, possibly even iterating until the Block has converged.

#### Blocked PR Input Format
'nodeid PageRank Neighbour-List'

#### Blocked PR Output Format
'nodeid PageRank'

#### Naive PR Code Description
###### PageRank.java
Driven class. Create Map-Reduce until it converges. The residual is set to be 0.001.

###### TrustMapper.java
Doing nothing, just pass data to Reducer
###### TrustReducer.java
PageRank iteration over Block. In-block iteration number is set to be 5. 

#### Blocked PR Converge
7 Passes until converge:
Pass 1: 0.0705805572899
Pass 2: 0.026460936968
Pass 3: 0.00969286486241
Pass 4: 0.00432837274331
Pass 5: 0.00185918862238
Pass 6: 9.515053339754535E-4

![image](https://raw.githubusercontent.com/git254/image/master/b-con.png)

## AWS EMR monitor 
![image](https://raw.githubusercontent.com/git254/image/master/AWS.png)

