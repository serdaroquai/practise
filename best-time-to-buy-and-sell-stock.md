# Best Time to Buy and Sell Stock (Easy)
https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

> Say you have an array for which the ith element is the price of a given stock on day i.  
> If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.  
> Note that you cannot sell a stock before you buy one.  
> 
> Example 1:
> 
> Input: [7,1,5,3,6,4]  
> Output: 5  
> Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.  
> Not 7-1 = 6, as selling price needs to be larger than buying price.
---
* Greedy, 
  * at each step check if profit is the best answer
  * price is a better minimum.
* prices must be at least length 2 in order to complete a transaction.
* Order of operations are important. You can't use a new minimum to calculate profit the moment you find it so start with profit then update minimum.
* Runtime O(n), Space O(1)

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length < 2) return 0;

    int minSoFar = prices[0];
    int profit = 0;

    for (int i=1; i< prices.length; i++){
        profit = Math.max(profit, prices[i] - minSoFar);
        minSoFar = Math.min(minSoFar, prices[i]);
    }

    return profit;
}
```
# Best Time to Buy and Sell Stock II (Easy)
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/

> Say you have an array for which the ith element is the price of a given stock on day i.  
>
> Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).  
>
> Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).  
>
> Example 1:  
>
> Input: [7,1,5,3,6,4]  
> Output: 7  
> Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.  
> Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.  
---
* If you have unlimited transactions, problem is reduced to finding the sum of all increasing prices.
* But we can not buy and sell at the same day. Actually we can since, buying and selling at the same day = keeping!

```java
public int maxProfit(int[] prices) {
    int sum = 0;
    for (int i=1;i<prices.length;i++) {
        sum += (prices[i-1] < prices[i]) ? prices[i] - prices[i-1] : 0; 
    }
    return sum;
}
```
