## Knapsack with repetitions (unlimited items)

Imagine a knapsack of `W=10kg`, and a list of items `[1,3,5]`. The key insight is: 
* If you take any item `Wn` out of the optimal knapsack of `W`, the resulting knapsack `W - Wn` must also be an optimal solution

> [x|x|xxx|xxxxx] W = 10, [1,1,3,5]  
> [x|x|xxx] Take 5 out: W = 5, [1,1,3] is optimal for size 5  
> [x|x|xxxxx] Take 3 out: W = 7, [1,1,5] is optimal for size 7  


## Coin change I to II

See the subtle mistake

for `amount=5, coins=[2,3]`

```java
dp[amount+1]
for (int i=0; i<=amount; i++) {
  for (int c: coins) {
    dp[i] += (i>=c) ? dp[i-c] : 0;
  }
}
```
will yield:

```
dp 0 1 2 3 4 5 
   1 0 1 1 1 2 
// dp[5] represents both 2+3 and 3+2
```

However when we switch the loop
```java
dp[amount+1]
for (int c: coins) {
  for (int i=0; i<=amount; i++) {
    dp[i] += (i>=c) ? dp[i-c] : 0;
  }
}
```
will yield:

```java
dp 0 1 2 3 4 5
   1 0 1 0 1 0  // first for coin 2
   1 0 1 1 1 1  // then for coin 3 
// dp[i] now represents 'using 2&3' aka using the first two coins
```
