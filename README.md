Decoupling, Formally

This article's goal is to demonstrate that to decouple is to minimize of the number of dependencies that connect Domains, as well as to minimize the change in this value given a change in the components of the system.

Use case: https://adrianjewell91.github.io/decoupler-website/

The begin, let there be a set B, a collection of Business Processes. The properties of the Business Processes are intentionally vague in order to generalize. The only requirement is that each Business Process be unique, because otherwise there would be no reason to decouple them. (This assumption rests on the idea that decouplable processes must be divisible, and identical process are not divisible. If identical processes are divisible, then they are ordered or enumerated in some manner that identifies them uniquely).

	B = { b_1, b_2, ... b_n }.



Next, let there be a set `D`, a collection of Dependencies, which express the direct relationships of Business Processes to one another. To generate it, map each Business Process to zero or more other Business Processes, defined by:

	D = { d ∈ B x B: b_1 R b_2 }. 
 

Note that `D ⊂ B x B` and `D ⊂ P(B)`, which is siginificant insofar as it shows that `D` is a subset of all possible combinations of two Business procceses, and also a subset of the complete set of all possible subsets (ie. the power set of B). It is also a directed relationship, in order to model real world dependencies.


Using `D`, we can create a set of Domains, called `M`: a collection of Domains. Given a maximum Domain `size`, the set `M` is constructed by transforming `D`. Note that there can be more than one valid set of Domains, (although we will not prove it). This transformation is described by two functions `h` and `g`:


	let h : D -> M, where

	M = { m ∈ P(B) : U m == B and g( m ) = 1 for each m }.

	let g : (B, D) -> { 0, 1 }, and g(m) = 1 if
		for each b ∈ m, there is a path to every other b` strictly inside of m (ie. through inter-Domain Dependencies, only).

The crucial function is `g`, which validates a subset of `D` to be a valid Domain.

From `M`, there exists a value called the `Total Coupling Metric`, which is the quantity of Dependencies that cross Domains. That is, it is the sum of the Dependencies that connect two Domains, instead of connecting Business Processes within a Domain:

 	let j : M -> N, where j(M) = [Summation over { m x m : m ∈ M } ] q(m1, m2), where

	q: (m1, m2) -> N, where q(m1, m2) = count(m1 ∩ m2).

The crucial function is `q` which counts how many Dependencies crosses between Domains.


Because there can be multiple valid mappings of the set `D` to an `M`, there exists a set `TC`, a collection all possible Total Coupling Metrics. The generation of `TC` is an onto function of a possible `M` to the range of possible total couplings, which a subset of the natural numbers, `N`:

	TC = { tc ∈ N : M exists and j(M) = tc }.


Therefore, the `Minimum Total Coupling` is mapped to by the optimal `M`. Formally, the `Minimum Total Coupling` is the infimum of the set `TC`:

	Minimum Total Coupling (MTC) = inf TC.


Thus finding the `Minimum Total Coupling` yields an optimally decoupled system. 

This is good, but it is not complete, because companies are always developing their members of `B`. Therefore, to optimally modify the set `B` of Business Processes, find the minimum change in the 'Total Coupling' given a change in `B`. This is equivalent to optimizing the increase of `B`, because in software development, it is basically the process of adding things. Therefore, the optimal decoupling strategy is the infimum of a set `C`, a collection of all possible changes to `TC` given any change to set `B`:

	let B' = { all possible incremental changes to B }.

	C = { all possible changes to TC, given additon of b' ∈ B' to B }.

	Optimal Decoupling Strategy = inf C.

Other ways to think about this are: finding the optimal `M`, or finding the best way to change `D`. 

Thus, there are two parts to the decoupling problem: finding an optimal decoupling, perhaps for the purposes of organizational efficiency, and an optimal decoupling that minimizes the increase in coupling. If the goal is to decouple a system in the optimal way, find the `Minimum Total Coupling`. Additionally, if the goal is to optimize the coupling given a change in `B`, then find `M` such that a change in `B` causes the minimal change in the `Total Coupling Metric`.  Note, the optimal `total coupling ∈ TC` may or may not be equivalent to the `Minimum Total Coupling`, but if it is, it would be the best situation because it solves both aspects of the decoupling problem at the same time.

Regarding the finding of `C`, note that many factors affect the membership of `B'`, so the process can be highly subjective.


Example. Given max Domain size = 3: 
![img](https://github.com/Adrianjewell91/decoupler-website/blob/main/Screenshot%202024-02-10%20at%2010.01.19%20AM.png)
