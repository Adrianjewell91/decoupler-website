# Enterprise Decoupling
https://adrianjewell91.github.io/decoupler-website/.

# Simple Example: 

### Premise

A SQL query was performing badly inside a GraphQL component but performing well using the SQL console. The challenge was to root cause and solve the problem, which was non-trivial because debugging was not yielding insight. 

The solution was to rewrite the query to be more DRY. In retrospect the query could obviously have been refactored, but this was not apparent at first glance because of the way that the problem presented itself.

### Explanation
Initially, the problem looked like this:

![Screenshot 2025-05-27 at 3 35 40 PM](https://github.com/user-attachments/assets/bd397ded-b7c8-4375-958e-2f43486ce86c)

But in reality, the problem really looked like this:

![Screenshot 2025-05-27 at 3 35 52 PM](https://github.com/user-attachments/assets/eacc093f-04df-44a2-b204-85d3eb0fb913)

Therefore the problem solving strategy was to converge on the optimal decoupling of `B` by understanding the true nature of the business processes. For the example, initially it looked as if the systems were identical except for the SQL, but that turned out to be false. For example, upon further investigation, the GQL component contained many additional dependencies, which divided it from the Azure components. This knowledge reworked the domain boundaries. 

The domain reworking reduced the total coupling from `4` to `2`. Mentally, this allowed the engineer to focus on the main problem, which was the SQL instead of wondering about why the SQL should have performed differently on two seemingly identialy configurations. 
