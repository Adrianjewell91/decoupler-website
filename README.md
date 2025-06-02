Let:

- $`B = \{b_1, b_2, \dots, b_n\}`$ be the set of business processes.
- $`D \subseteq B \times B`$ be the set of dependencies between business processes.
- $`C \subseteq D`$ be the set of **intra-domain** dependencies.
- $`s \in \mathbb{N}`$, the max domain size.

Let:

- $`f: (s, D) \to C`$ be a mapping that identifies which dependencies are inter-domain.
- $`M \subseteq \mathcal{P}(B)`$ be a set of subsets of $`B`$, representing domain groupings. The existence of this set is implied by $`f`$.

**Requirements**:
- For each domain $` m \in M `$, all business processes within $` m `$ must be reachable **only through intra-domain dependencies**, i.e., no dependency path between two processes in $` m `$ may traverse any process outside $` m `$. All such paths must exclusively use dependencies in $` D \setminus C `$.
- $`\cap M = \{ \}`$, representing that each domain must be unique in its membership.


**Total Coupling**:
$`
\text{Total Coupling} = |C|
`$

**Goal**:
$`
\min |C|
`$

That is, minimize the Total Coupling to reduce inter-domain dependencies and achieve better modularity across business domains.



---
Example. Given $`s`$ = 3: 
![img](https://github.com/Adrianjewell91/decoupler-website/blob/main/Screenshot%202024-02-10%20at%2010.01.19%20AM.png)


**Notes**:

The goal of this system is to define of the meaning of being "decoupled". The motivation was the observation that "decoupled" was a highly subjective term in business discussions. The hope is that this definition is useful, despite being highly abstract. A number of examples will attempt to apply the framework.


In short, decoupling is the optimization of dependencies between business domains. This optimization can have two parts. The first part is the minimization of the number of dependencies that connect business domains, and the second part is the minimization of the increase in this number given a change to the components of the system.


Define set `B` as a collection of business processes. The properties of the Business Processes are intentionally vague for the purpose of generalization. The only requirement is that each Business Process be unique, because otherwise there would be no reason to decouple them. This assumption rests on the idea that decouplable processes must be divisible, and identical process are not divisible. If identical processes are divisible, then they are ordered or enumerated in some manner that identifies them uniquely.


Next, define set `D` as a collection of dependencies, which are the direct relationships of business processes to one another. To generate it, map each business process to zero or more other business processes.


Note that `D` is a subset of the Cartesian Product, aka. `D ⊂ B x B`,  which is siginificant insofar as it shows that `D` is a subset of all possible combinations of two business procceses. 


Using `D` and `B`, define set `M` as a collection of domains. To construct M given a maximum size `s`, group the members of `B` into a collection of max size `s` whose union is `B` and intersection is an empty set. Note that there can be more than one valid set of domains. 

This is to say, for each process in a domain, there should be a path from each member to every other member in the same domain without having to leave the boundary of the domain. This definition is based opon the intuition that domain processes are connected in more ways than they connected to processes outside of the domain.


Based on `M`, there exists the `Total Coupling`, which is the quantity of dependencies that connect domains. That is, it is the sum of the dependencies that connect two domains, instead of connecting business processes within a domain:

Because there are potentially multiple mappings of `D` to `M`, there exists a set `T` of all possible Total Couplings. The calculation of `T` is an onto (but not necessarily 1-1) function of possible `M` to the natural numbers.

Therefore, the `Minimum Total Coupling` is found in the optimal `M`. Formally, the `Minimum Total Coupling` is the infimum of the set `T`.

Thus, finding the `Minimum Total Coupling` yields an optimally decoupled system. 

However, business evolve, and therefore the constitution of `B` evolves. Therefore, to remain decoupled is to find the minimum change in the 'Total Coupling' given a change in `B`. In software development, where any change is essentially an addition, this is equivalent to optimizing the increase of in the membership of `B`. Therefore, the optimal decoupling strategy is the infimum of a set `C`, a collection of all possible changes to `TC` given any change to set `B`:

Other ways to think about this are: finding the optimal `M`, or finding the best way to change `D`. 

In conclusion, decoupling considers the present and the future, but they need not both be required. That is, finding an optimal decoupling, perhaps for the purposes of organizational efficiency, and an optimal decoupling that minimizes the increase in coupling, can be done separately. If the goal is to decouple a system in the optimal way, find the `Minimum Total Coupling`. Additionally, if the goal is to optimize the coupling given a change in `B`, then find `M` such that a change in `B` causes the minimal change in the `Total Coupling Metric`.  

Note, the optimal `total coupling ∈ TC` may or may not be equivalent to the `Minimum Total Coupling`, but if it is, it would be the best situation because it solves both aspects of the decoupling problem at the same time.

Regarding the finding of `C`, note that many factors affect the membership of `B'`, so the process can be highly subjective.
