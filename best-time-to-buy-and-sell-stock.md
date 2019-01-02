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
* Runtime O(n), Space O(1)

```java
public int maxProfit(int[] prices) {
    int sum = 0;
    for (int i=1;i<prices.length;i++) {
        sum += (prices[i-1] < prices[i]) ? prices[i] - prices[i-1] : 0; 
    }
    return sum;
}
```

# Best Time to Buy and Sell Stock III (Hard)
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/

> Say you have an array for which the ith element is the price of a given stock on day i.  
>
> Design an algorithm to find the maximum profit. You may complete at most two transactions.  
>
> Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).  
>
> Example 1:  
>
> Input: [3,3,5,0,0,3,1,4]  
> Output: 6  
> Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.  
> Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.  
---
* Mighty dynamic programming
* The point is to buy low and sell high but we need to somehow link two transactions together.
* Stat by building the best result from bottom up, (start with only one price and add until prices list is depleted)
* The idea is to link every sell with previous buy and every buy with previous sell. In this case we have only two
  * The higher you sell the stock, the more money you have to buy next stock
  * The lower you buy the next stock, the higher you sell it.
* Runtime O(n), space O(1)

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length < 1) return 0;

    int buyOne=Integer.MAX_VALUE;
    int sellOne=0;
    int buyTwo=Integer.MAX_VALUE;
    int sellTwo=0;

    for (int p : prices){
        buyOne = Math.min(buyOne, p); // buy low
        sellOne = Math.max(sellOne,p-buyOne); // sell high (buying first stock cheaper helps)
        buyTwo = Math.min(buyTwo, p-sellOne); // buy low (profit of first stock helps)
        sellTwo = Math.max(sellTwo, p-buyTwo); // sell high (buying second stock cheaper helps)
    }

    return sellTwo;
}
```

# Best Time to Buy and Sell Stock IV (Hard)
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/

> Say you have an array for which the ith element is the price of a given stock on day i.  
> 
> Design an algorithm to find the maximum profit. You may complete at most k transactions.  
> 
> Note:  
> You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).  
> 
> Example 1:  
> 
> Input: [2,4,1], k = 2  
> Output: 2  
> Explanation: Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.  
>
> Example 2:  
>
> Input: [3,2,6,5,0,3], k = 2  
> Output: 7  
> Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4.  
> Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.  
---

* This is a generalization of previous problem.
* Higher you sell previous stock, cheaper overall buy for current stock. `buy[k] = Math.min(buy[k], p - sell[k-1])` where k is the k'th transaction. (buy[k] is linked with sell[k-1])
* Lower you buy the current stock, more profit you make by selling. `sell[k] = Math.max(sell[k], p - buy[k])`. (sell[k] is lnked with buy[k])
* Edge case for performance (and to prevent memory overflow for k = 10000000): 
  * Since for every 2 price in prices we only need one tx, a point to keep in mind is if `K > prices.length / 2` then we have unlimited transactions. So problem degrades to maxProfit (Buy and sell stock II) problem which is an easy problem with unlimited tx.
* Runtime O(kn) where k is number of tx, Space O(k)

```java
public int maxProfit(int K, int[] prices) {
    if (prices.length < 2) return 0;

    // K = Math.min(K, prices.length / 2); // useless to have 100 tx for say 5 prices. 
    if (K > prices.length / 2) return maxProfit(prices); // problem becomes maxProfit (unlimited Transactions)
    
    int[] buy = new int[K+1];
    int[] sell = new int[K+1];

    Arrays.fill(buy, prices[0]);    // when there is only one price you can buy
    // Arrays.fill(sell, 0);        // can't profit by buying and selling all at same price

    for (int p: prices) {
        for (int k=1; k<K+1; k++) {
            buy[k] = Math.min(buy[k], p - sell[k-1]);   // best buy with k'th tx (selling k-1'th tx high helps lowering cost)
            sell[k] = Math.max(sell[k], p - buy[k]);    // best sell with k'th tx (buying k'th tx low helps maxing profit)
        }
    }

    return sell[K];
}
```
## Best Time to Buy and Sell Stock With Tx Fee (Medium)
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/

> Your are given an array of integers prices, for which the i-th element is the price of a given stock on day i; and a non-negative integer fee representing a transaction fee.  
>
> You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)  
>
> Return the maximum profit you can make.  
>
> Example 1:  
> Input: prices = [1, 3, 2, 8, 4, 9], fee = 2  
> Output: 8  
> Explanation: The maximum profit can be achieved by:  
> Buying at prices[0] = 1  
> Selling at prices[3] = 8  
> Buying at prices[4] = 4  
> Selling at prices[5] = 9  
> The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.  
> Note:  
>
> 0 < prices.length <= 50000.  
> 0 < prices[i] < 50000.  
> 0 <= fee < 50000.  

* Have two valid states, either we have stock or no stock.
  * To have stock, either we keep it, or buy it at current price + fee, whichever is better
  * To sell stock, either we don't buy, or sell what we bought, whichever is better
* Runtime O(n), Space O(1)

```java
public int maxProfit(int[] prices, int fee) {
    
    int stock = -prices[0]-fee;
    int profit =0;

    for (int i=1; i<prices.length; i++) {
        stock = Math.max(stock, profit-prices[i]-fee);
        profit = Math.max(profit, stock + prices[i]);
    }
    
    return profit;
}
```
