## Using Node2Vec

### Given a log of SQL queries against a database: 

1. Build a graph of relationship between clients and tables. 
2. Embedding the graph using node2vec.
3. Visualize the embeddings and known labels.
	- Client or Table.
	- Inside or Outside the largest connected component.


### Notes:

- There are two graph-building flows:
	- JOINs connect Clients to Tables.
	- FROMS connect Clients to Tables, and JOINS connect Tables to Tables.
	
	
- The known labels are build from observations of graph, which are:
	- That there is one large connected component and many tiny ones.
	- That there many unique clients (where a client == a unique sql hash).



### Input:
A csv of sql_hashes and queries, taken from solrwind db analyzer. Roughly 18K queries (before deduplication).

### Results:

Graph (using cosmograph):

<img width="1310" height="682" alt="Screenshot 2025-08-01 at 12 24 55 PM" src="https://github.com/user-attachments/assets/a5883772-1a43-4365-9c9c-f94aefb07e0c" />

Visualizations:

<img width="932" height="800" alt="Screenshot 2025-08-01 at 12 25 11 PM" src="https://github.com/user-attachments/assets/2d7c13b5-c4d6-4b40-b2cb-0aaeffea26c4" />

<img width="946" height="796" alt="Screenshot 2025-08-01 at 12 25 25 PM" src="https://github.com/user-attachments/assets/24760ef5-feb9-41aa-b520-00fc93ba211b" />


## Developer Velocity

One problem in Developer Velocity is the need to spend a lot of time writing and testing code because there are too many dependencies that might break when adding a new feature or a fixing a bug. At this point, the developer must decide on a course of action: whether to take the risk and not test, whether to write the tests, whether to refactor, or try to avoid the dependencies all together and write duplicate/new code, among other options. 

The below figures shows a graph of dependencies and three possible routes to integrate new dependencies. In the first example, dependencies are accessed individually without any regard for the other dependencies. In the second example, some judiciousness is taking to group dependencies into fewer interfaces, and lastly, the other extreme is taken where a single new interface is built that captures completely all the dependencies.

When would one strategy be preferrable over another? That is a complicated question. For example, if the domain teams know the best way to expose their dependencies then it might be easy to go with option 2. However, if that information is not known then option 1 or 3 might be better. Option 1 might be better if there will be a natural emergence of new domains with the new interfaces. Option 3 might be better if the system is siloable and won't be depended upon in the future. 

<img width="997" height="4011" alt="image" src="https://github.com/user-attachments/assets/0d5ae631-11bc-4af1-84dc-1ac7a6bdf5bf" />


## Modularity - Graph Theory

One algorithm is Modularity optimization, which Modularity is the ratio of edges within communities over the edges that would be within the community if edges were distributed at random.  See: https://pmc.ncbi.nlm.nih.gov/articles/PMC1482622/#sec1. 

## Densely Connected Graphs

When $`D = B \times B`$, and only two domains are required, then the minimum total coupling can be calculated according to the size of $`|B|`$. 

Let:

- $`n = |B|`$.

Then: 

$$\text{min Total Coupling} = \dfrac{n+1}{2} * \(n - \dfrac{n+1}{2}\) $$, (using integer arithemtic).

Example:

<img src="https://github.com/user-attachments/assets/de02b511-3b73-4d4a-9f27-c0a2d9f99e3c" width="250" height="700">


## A Simple Visual
Example. Given $`s`$ = 3: 
![img](https://github.com/Adrianjewell91/decoupler-website/blob/main/Screenshot%202024-02-10%20at%2010.01.19%20AM.png)


## Organizational
This article attempts to apply the framework to a theoretical large organization that needs to rearchitect its domain boundaries at scale. It also discusses the operational challenges of such a task. 
https://adrianjewell91.github.io/decoupler-website/.

## A Simple Example #1

### Premise

A complex SQL query performed badly inside a GraphQL system but performed well using the SQL console. The challenge was to align the performance and get it query to perform well in GQL, which was non-trivial because debugging logs did not yield insight. 

The solution was not hard but finding the solution was indeed hard. However this was not immediately apparent because the focus was in the wrong place. Once the focus was right, the insight was easy, which was that the query was not DRY and therefore could be rewritten. In retrospect, the query could obviously have been refactored, but this was not apparent at first glance.

### Explanation
Initially, the problem looked like this:

![Screenshot 2025-05-27 at 3 35 40 PM](https://github.com/user-attachments/assets/bd397ded-b7c8-4375-958e-2f43486ce86c)

But in reality, the problem really looked like this:

![Screenshot 2025-05-27 at 3 35 52 PM](https://github.com/user-attachments/assets/eacc093f-04df-44a2-b204-85d3eb0fb913)

Therefore the problem solving strategy was to converge on the optimal decoupling of `B` by understanding the true nature of the business processes. For the example, initially it looked as if the systems were identical except for the SQL, but that turned out to be false. For example, upon further investigation, the GQL component contained many additional dependencies, which divided it from the Azure components. This knowledge reworked the domain boundaries. 

The domain reworking reduced the total coupling from `4` to `2`. Mentally, this allowed the engineer to focus on the main problem, which was the SQL instead of wondering about why the SQL should have performed differently on two seemingly identical configurations. 

## A Less Simple Example #2

In a data aggregation pipeline, there were data consistency issues. This was caused by an old library causing memory leaks, which caused the consumers to slow to a halt. The initial problem was that the pipeline was hard to reason about because of the number of consumers and sources.

The solution was the refactor into the 4 constituent consumers then debug the highest traffic. Once the library was removed, it was easy to apply the pattern to the remaining consumers.

In this way, the system went from highly coupled (total coupling of 3) to not coupled anymore (total coupling of 0).

![image](https://github.com/user-attachments/assets/ff315a56-b881-459d-b3d5-832a918b75aa)


## A Less Simple Example #3

A legacy data synchronization pipeline failed often due to depending heavily on shared models / infrastructure, and using resource-intensive batch queries. This in indicated by the heavy red lines, which represent the multitude of dependencies on other jobs that use the same models.

The solution was to rewrite the job as a service orchestrator, leveraging new domain interfaces and event streams, reducing the coupling on shared infra, and creating new dependencies on single-point-of-entry into the required domains.

New dependencies were created but because they were the single point of entry, the total coupling decreased. This is because the legacy shared model was a dependency that was orders of magnitude greater than the new domain interfaces.

![image](https://github.com/user-attachments/assets/514900dc-9bac-46b6-9323-4704e009f498)

