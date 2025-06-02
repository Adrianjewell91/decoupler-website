## Visual
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

## A Simpler Example #2

## A Simpler Example #3
