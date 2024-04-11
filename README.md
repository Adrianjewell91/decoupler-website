Decoupling, Formally

This article's purpose is to demonstrate that the Decoupling Problem is an optimization of a value call the Minimun Total Coupling.


The begin, let there be a set B, a collection of Business Processes.

	B = { b_1, b_2, ... b_n }.



Next, let there be a set `D`, a collection of Dependencies, which express the direct relationships of Business Processes to one another. To generate it, map each Business Process to zero or more other Business Processes, defined by:

	D = { d ∈ B x B: b_1 R b_2 }. 
 

Note that `D ⊂ B x B` and `D ⊂ P(B)`, which is siginificant insofar as it shows that D is a subset of all possible combinations of two Business procceses, and also a subset of the complete set of all possible subsets (ie. the power set of B).


`D` can be grouped into a set of Domains, denoted as a set called `M`, a collection of Domains. Given a maximum Domain `size`, the set `M` is constructed by transforming `D`. Note that there can be more than one valid set of Domains, (although we will not prove it here). This transformation is described by two functions `h` and `g`, and the set `M`:


	let h : (B, D) -> M, where

	M = { m ∈ P(B) : U m == B and g( m ) = 1 }, and

	let g : P(B) -> { 0, 1 }, and g(m) = 1 if
		for each Domain of m ⊂ B, there is a path from every b in the Domain to every other b' in the same Domain through the inter-Domain Dependencies, only.

The crucial function is `g`, which validates a subset of `D` to be a valid Domain.

Now, there exists a value called the `Total Coupling Metric`, which is the quantity of Dependencies that cross Domains. In other words, it sums the Dependencies that connect two Domains instead of connecting Business Processes within a Domain:

 	let j : M -> N, where j(M) = [Summation over { m x m : m ∈ M } ] q(m1, m2), where

	q: (m1, m2) -> N, where
		q(m1, m2) = count(m1 ∩ m2).

The crucial function is `q` which counts how many Dependencies crosses between Domains.


Because there can be multiple groupings of the set `D` into Domains, there will exist a set `TC`, a collection all possible Total Couplings. The generation of `TC` is an onto function of a possible `M` to the range of possible total couplings, which a subset of the natural numbers, `N`:

	TC = { tc ∈ N : M exists and j(M) = tc }.


Finding the `Minimum Total Coupling` gives the optimal domain groupings. Formally, the `Minimum Total Coupling` is the infimum of the set `TC`:

	Minimum Total Coupling (MTC) = inf TC.



To add or modify the set `B` of Business Processes in the optimal way, find the change in `D` given a change in `B` that minimizes the change in the 'Total Coupling Metric'. This is essentially equivalent to optimizing the increase of `B`, because in Software Development, most optimization is a process of adding things, (at least to start). Therefore, the optimal decoupling strategy is the infimum of all possible changes to `TC` given any change to `B`, denoted as set `C`:

	let B' = { all possible incremental changes to B }.

	C = { all possible changes in TC, given additon of b' ∈ B' to B }.

	Optimal Decoupling Strategy = inf C.


In Summary, if the goal is to Decouple a system in the optimal way, find the `Minimum Total Coupling`. Additionally, if the goal is to optimize the coupling given a change in `B`, then find the optimal decoupling situation such that a change in `B` causes the minimal change in the `Total Coupling Metric ∈ TC`.  Note, the optimal `tc ∈ TC` may or may not be equivalent to the `MTC` in this situation.

Regarding the finding of `C`, note that many factors affect the membership of `B'`, so the process can be highly subjective.


Example. Given max Domain size = 3: 
![img](https://github.com/Adrianjewell91/decoupler-website/blob/main/Screenshot%202024-02-10%20at%2010.01.19%20AM.png)
