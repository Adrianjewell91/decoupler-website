Decoupling, Formally

This article's purpose is to demonstrate that the Decoupling Problem is an optimization on a metric call the Minimun Total Coupling.


The begin, let there be a set B, a collection of Business Processes.

	B = { b_1, b_2, ... b_n }.



Next, let there be a set D, collection of Dependencies, which express the direct relationships of Business Processes to one another. To generate it, map each Business Process to zero or more other Business Processes, defined by:

	D = { d ∈ B x B: b_1 R b_2 }. 
 

Note that `D ⊂ B x B` and `D ⊂ P(B)`, which is siginificant insofar as showing that D is a subset of all possible combinations of two Business procceses, and also a subset of the complete set of all possible subsets (ie. the power set of B).


The set `D` can be grouped into a set of Domains, denoted as a set called `M`, a collection of Domains. Given a maximum Domain `size`, the set M is constructed by transforming D. Note that there can be more than one valid set of Domains, (although we will not prove it here). This transformation is described by two functions `h` and `g`, and the set `M`:


	let h : (B, D) -> M, where

	M = { m ∈ P(B) : U m == B and g( m ) = 1 }, and

	let g : P(B) -> { 0, 1 }, and g(m) = 1 if
		for each Domain of m ⊂ B, there is a path from every b in the Domain to every other b' in the same Domain through the inter-Domain Dependencies, only.

The crucial function is `g`, which validates a subset of `D` to be a valid Domain.

Now, there exists a value called the `Total Coupling metric`, which is the quantity of Dependencies that cross Domains. In other words, it sums the Dependencies that connect two Domains instead of connecting Dependencies within a Domain:

 	let j : M -> N, where j(M) = [Summation over { m x m : m ∈ M } ] q(m1, m2), where

	q: (m1, m2) -> N, where
		q(m1, m2) = [Summation over each b in m1] {b'} U m2  if m1 != m2 else return 0.



The set of all possible Total Couplings is a 1-to-1, but not onto, function of M to the natural numbers N:

	TC = { tc ∈ N :  m ∈ M and j(m) = tc }.



Finding the Minimum Total Coupling gives the optimal Domain Structure. Think of the optimal Domain Structure as reducing the Dependencies between Domains. Thus, the Minimum Total Coupling is the infimum of the set of all possible Total Couplings:

	Minimum Total Coupling (MTC) = inf TC.



Finding the smallest change in D given an increase in B according to an order give the optimal Decoupling. Think of Decoupling as optimizing for the requirements to increase the size of B. Therefore, the optimal Decoupling is the infimum of all possible changes to D given any B':

	dD / dB = { dd/db_n ∈ N : k(B') = dd/db_B' }, where

	k : B' -> N, and

	B' = { b_n : n ∈ N and b !∈ B }, therefore

	Maximum Decoupling = inf dD / dB.


Regarding the solution of B', note that many factors affect the membership of B' such as the time frame under consideration, so the optimization of D can be highly subjective.

