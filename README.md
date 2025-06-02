A formal definition of Decoupling:

Let $\( B \)$, $\( D \)$, and $\( M \)$ be sets such that:

\[
B = $\{1, 2, 3, \ldots, n\}$
\]

\[
D $\subseteq B \times B$
\]

\[
M $\subseteq \mathcal{P}(B)$
\]

Where:
- \( B \) is a set labeled from 1 to \( n \),
- \( D \) is a subset of the Cartesian product \( B $\times$ B \),
- \( M \) is a subset of the power set of \( B \), denoted \( $\mathcal{P}(B)$ \).

---
Example. Given max Domain size = 3: 
![img](https://github.com/Adrianjewell91/decoupler-website/blob/main/Screenshot%202024-02-10%20at%2010.01.19%20AM.png)




The goal of this article is to formally define of the word "decouple". In essence, it is the optimization of dependencies between business domains. This optimization can have two parts. One part is the minimization of the number of dependencies that connect business domains, and the other part is the minimization of the increase in this number, given a change to the components of the system.


Define set `B` as a collection of Business Processes. The properties of the Business Processes are intentionally vague for the purpose of generalization. The only requirement is that each Business Process be unique, because otherwise there would be no reason to decouple them. This assumption rests on the idea that decouplable processes must be divisible, and identical process are not divisible. If identical processes are divisible, then they are ordered or enumerated in some manner that identifies them uniquely.

	B = { b_1, b_2, ... b_n }.



Next, define set `D` as a collection of Dependencies, which are the direct relationships of Business Processes to one another. To generate it, map each Business Process to zero or more other Business Processes, defined by:

	D = { d ∈ B x B: b_1 R b_2 }. 
 

Note that `D` is (sort of) a subset of the Cartesian Product, aka. `D ⊂ B x B`,  which is siginificant insofar as it shows that `D` is a subset of all possible combinations of two Business procceses. 


Using `D` and `B`, define set `M` as a collection of Domains. To construct M given a maximum size `s`, group the members of `B` into a collection of sets of max size `s` whose union is `B` and intersection is an empty set. Note that there can be more than one valid set of Domains. This transformation is described by two functions `h` and `g`:


	let h : D -> M, where

	M = { m ∈ P(B) : U m == B and g( m ) = true }.

	let g : (B, D) -> { 0, 1 }, and g(m) = true if
		for each member of set b ∈ m, there is a path to every other member in b using only dependencies in D.

Which is to say, for each process in a Domain, there should be a dependency-path from each process to every other member in the same domain without having to leave the boundary of the domain. This definition is based opon the intuition that domain processes are connected in more ways than they connected to processes outside of the Domain.

The crucial function is `g`, which essentially is validating a subset of `D` to be a valid Domain.

Based on `M`, there exists the `Total Coupling Metric`, which is the quantity of Dependencies that cross Domains. That is, it is the sum of the Dependencies that connect two Domains, instead of connecting Business Processes within a Domain:

 	let j : M -> N, where j(M) = [Summation over { m x m : m ∈ M } ] q(m1, m2), where

	q: (m1, m2) -> N, where q(m1, m2) = count(dependencies in `D` that connect m1 and m2).

The crucial function is `q` which counts how many Dependencies crosses between Domains.

Because there can be multiple valid mappings of the set `D` to an `M`, there exists a set `TC`, a collection all possible Total Coupling Metrics. The calculation of `TC` is an onto (but not necessarily 1-1) function of possible configurations of `M` to the range of possible total couplings, which is a subset of the natural numbers, `N`:

	TC = { tc ∈ N : M exists and j(M) = tc }.


Therefore, the `Minimum Total Coupling` is found in the optimal `M`. Formally, the `Minimum Total Coupling` is the infimum of the set `TC`:

	Minimum Total Coupling (MTC) = inf TC.


Thus, finding the `Minimum Total Coupling` yields an optimally decoupled system. 

However, business evolve, and therefore also evolving is the membership of `B`. Therefore, to remain decoupled is to find the minimum change in the 'Total Coupling' given a change in `B`. In software development, where any change is essentially an addition, this is equivalent to optimizing the increase of in the membership of `B`. Therefore, the optimal decoupling strategy is the infimum of a set `C`, a collection of all possible changes to `TC` given any change to set `B`:

	let B' = { all possible incremental changes to B }.

	C = { all possible changes to TC, given additon of b' ∈ B' to B }.

	Optimal Decoupling Strategy = inf C.

Other ways to think about this are: finding the optimal `M`, or finding the best way to change `D`. 

In conclusion, decoupling considers the present and the future, but they need not both be required. That is, finding an optimal decoupling, perhaps for the purposes of organizational efficiency, and an optimal decoupling that minimizes the increase in coupling, can be done separately. If the goal is to decouple a system in the optimal way, find the `Minimum Total Coupling`. Additionally, if the goal is to optimize the coupling given a change in `B`, then find `M` such that a change in `B` causes the minimal change in the `Total Coupling Metric`.  

Note, the optimal `total coupling ∈ TC` may or may not be equivalent to the `Minimum Total Coupling`, but if it is, it would be the best situation because it solves both aspects of the decoupling problem at the same time.

Regarding the finding of `C`, note that many factors affect the membership of `B'`, so the process can be highly subjective.
