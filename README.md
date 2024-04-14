Decoupling, Formally

This article's goal is to demonstrate that to decouple is to minimize of the number of dependencies that connect Domains, as well as to minimize the change in this value given a change in the components of the system.


The begin, let there be a set B, a collection of Business Processes. The properties of the Business Processes are intentionally vague in order to generalize the concept. The only requirement is that they be unique, because otherwise there would be no reason to decouple them. (This assumption rests on the idea that decouplable processes must be divisible, and identical process are not divisible. If identical processes are divisible, then they are ordered or enumerated in some manner that identifies them uniquely).

	B = { b_1, b_2, ... b_n }.



Next, let there be a set `D`, a collection of Dependencies, which express the direct relationships of Business Processes to one another. To generate it, map each Business Process to zero or more other Business Processes, defined by:

	D = { d ∈ B x B: b_1 R b_2 }. 
 

Note that `D ⊂ B x B` and `D ⊂ P(B)`, which is siginificant insofar as it shows that D is a subset of all possible combinations of two Business procceses, and also a subset of the complete set of all possible subsets (ie. the power set of B).


`D` can be grouped into a set of Domains, denoted as a set called `M`, a collection of Domains. Given a maximum Domain `size`, the set `M` is constructed by transforming `D`. Note that there can be more than one valid set of Domains, (although we will not prove it here). This transformation is described by two functions `h` and `g`:


	let h : D -> M, where

	M = { m ∈ P(B) : U m == B and g( m ) = 1 }.

	let g : (B, D) -> { 0, 1 }, and g(m) = 1 if
		for each b ∈ m, there is a path to every other b` strictly inside of m (ie. through inter-Domain Dependencies, only).

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



To optimally modify the set `B` of Business Processes, find the minimum change in the 'Total Coupling' given a change in `B`. This is equivalent to optimizing the increase of `B`, because in software development, optimization is generally a process of adding things. Therefore, the optimal decoupling strategy is the infimum of a set `C`, a collection of all possible changes to `TC` given any change to set `B`:

	let B' = { all possible incremental changes to B }.

	C = { all possible changes to TC, given additon of b' ∈ B' to B }.

	Optimal Decoupling Strategy = inf C.

Other ways to think about this are: finding the optimal `M`, or finding the best way to change `D`. 

To summarize, if the goal is to decouple a system in the optimal way, find the `Minimum Total Coupling`. Additionally, if the goal is to optimize the coupling given a change in `B`, then find `M` such that a change in `B` causes the minimal change in the `Total Coupling Metric`.  Note, the optimal `total coupling ∈ TC` may or may not be equivalent to the `Minimum Total Coupling`, although would be the best situation because it solves both aspects of the decoupling problem.

Regarding the finding of `C`, note that many factors affect the membership of `B'`, so the process can be highly subjective.


Example. Given max Domain size = 3: 
![img](https://github.com/Adrianjewell91/decoupler-website/blob/main/Screenshot%202024-02-10%20at%2010.01.19%20AM.png)
