# Best Time to Buy and Sell Stock

## Problem
Given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day, you want to maximize your profit by choosing a single day to buy one stock and a different day in the future to sell that stock.

Return the maximum profit you can achieve. If you cannot achieve any profit, return `0`.

### Example 1:
```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
```

### Example 2:
```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: No transaction is done, max profit = 0.
```

### Constraints:
- 1 <= prices.length <= 10^5
- 0 <= prices[i] <= 10^4

---

## Approach 1: Brute Force (O(n^2))
### Intuition
We can check every pair of days to find the maximum profit.

### Code
```kotlin
fun maxProfit(prices: IntArray): Int {
    var maxProfit = 0
    for (i in prices.indices) {
        for (j in i + 1 until prices.size) {
             val profit = prices[j] - prices[i]
             if (profit > maxProfit) {
                maxProfit = profit
            }
        }
    }
    return maxProfit
}
```
### Time Complexity: O(n^2)
### Space Complexity: O(1)

### Dry Run (Brute Force)
| i  | j  | prices[i] | prices[j] | Profit     | maxProfit |
|----|----|-----------|-----------|------------|-----------|
| 0  | 1  | 7         | 1         | -6         | 0         |
| 0  | 2  | 7         | 5         | -2         | 0         |
| 0  | 3  | 7         | 3         | -4         | 0         |
| 0  | 4  | 7         | 6         | -1         | 0         |
| 0  | 5  | 7         | 4         | -3         | 0         |
| 1  | 2  | 1         | 5         | 4          | 4         |
| 1  | 3  | 1         | 3         | 2          | 4         |
| 1  | 4  | 1         | 6         | 5          | 5         |
| 1  | 5  | 1         | 4         | 3          | 5         |
| 2  | 3  | 5         | 3         | -2         | 5         |
| 2  | 4  | 5         | 6         | 1          | 5         |
| 2  | 5  | 5         | 4         | -1         | 5         |
| 3  | 4  | 3         | 6         | 3          | 5         |
| 3  | 5  | 3         | 4         | 1          | 5         |
| 4  | 5  | 6         | 4         | -2         | 5         |

---

## Approach 2: Optimal (O(n))
### Intuition
We keep track of the minimum price so far and update the maximum profit at each step.

### Code
```kotlin
fun maxProfit(prices: IntArray): Int {
    var minPrice = Int.MAX_VALUE
    var maxProfit = 0

    for (price in prices) {
        if (price < minPrice) {
            minPrice = price
        } else {
            val profit = price - minPrice
            if (profit > maxProfit) {
                maxProfit = profit
            }
        }
    }

    return maxProfit
}
```
### Time Complexity: O(n)
### Space Complexity: O(1)

### Dry Run (Optimal)
| Day | Price | minPrice | maxProfit |
|-----|-------|----------|-----------|
| 1   | 7     | 7        | 0         |
| 2   | 1     | 1        | 0         |
| 3   | 5     | 1        | 4         |
| 4   | 3     | 1        | 4         |
| 5   | 6     | 1        | 5         |
| 6   | 4     | 1        | 5         |

Final maxProfit = 5
