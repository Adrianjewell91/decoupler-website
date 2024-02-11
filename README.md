Decoupling, Formally

The purpose of this section is to demonstrate formally that the Decoupling Problem is an optimization.


The set B is a collection of Business Processes.

	B = { b_1, b_2, ... b_n }.



A set of Dependencies expresses the direct relationships of Business Processes to one another. To generate it, map the Business Process to zero or more other Business Processes, defined by:

	D = { d ∈ B x B: b_1 R b_2 }, D ⊂ B x B, D ∈ P(B).


Given a maximum size of a Domain, A set of Mappings is a transformation of D into a set of sets of Domains, where there is more than one valid set of Domains, defined as:



	h : B, D -> M, where

	M = { m ⊂ P(B) : U m == B and g( m ) = 1 }, where

	g : P(B) -> { 0, 1 }, and 	g(m) = 1, if and only if:
		for each Domain of m ⊂ B, there is a path from every b in the Domain to every other b' in the same Domain through the inter-Domain Dependencies only.



There exists a Total Coupling metric, defined as the summation of the dependencies between each Domain in a Mapping to the other Domains in the same Mapping, written as:

 	j : M -> N, where j(m) = [Summation over { m x m : m ∈ M } ] q(m_domain_1, m_domain_2), where

	q: (domain_1, domain_2) -> N, where
		q(do_1, do_2) = [Summation over each b in do_1] b_n_do_1 U domain_2  if do_1 1= do_2 else return 0.



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

