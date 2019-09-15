## Knapsack with repetitions (unlimited items)

Imagine a knapsack of `W=10kg`, and a list of items `Wn=[1,3,5]`  
The key insight is: If you take any item out of the knapsack, the resulting knapsack must be an optimal solution

> [x|x|xxx|xxxxx] W = 10, [1,1,3,5]  
> [x|x|xxx] Take 5 out: W = 5, [1,1,3] is optimal for size 5  
> [x|x|xxxxx] Take 3 out: W = 7, [1,1,5] is optimal for size 7  
