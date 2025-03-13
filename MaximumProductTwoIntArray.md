## Find the Maximum Product of Two Integers in the Array

Given an array of integers, we need to find the maximum product of two integers in the array

Input: arr[] = {4, 6, 8, -5, -4, 4}  
Output: 48, {6,8}  
Explanation: Maximum product is 48, which is obtained by 
multiplying the numbers 6 and 8 of the array.

Let's solve this step by step in an intuitive way by exploring multiple approaches from brute force (O(N²)) to optimal (O(N)) while explaining the thought process.
Step 1: Understanding the Problem

We need to find two integers in the array whose product is maximum and return both the product and the numbers.
Key Observations:

  - The maximum product can come from:
        - Two largest positive numbers. (e.g., 6 × 8 = 48)
        - Two smallest (negative) numbers. (e.g., (-5) × (-4) = 20, since multiplying negatives gives a positive)
  - Our task is to compare both possibilities and return the maximum.

### Approach 1: Brute Force (O(N²)) - Checking All Pairs
Thought Process:

    Try all pairs (i, j) in the array.
    Compute their product.
    Keep track of the maximum product and the corresponding pair.

```
fun maxProductBruteForce(arr: IntArray): Pair<Int, Pair<Int, Int>> {
    var maxProduct = Int.MIN_VALUE
    var pair = Pair(0, 0)

    for (i in arr.indices) {
        for (j in i + 1 until arr.size) {
            val product = arr[i] * arr[j]
            if (product > maxProduct) {
                maxProduct = product
                pair = Pair(arr[i], arr[j])
            }
        }
    }

    return Pair(maxProduct, pair)
}

// Testing the function
fun main() {
    val arr = intArrayOf(4, 6, 8, -5, -4, 4)
    val (maxProduct, pair) = maxProductBruteForce(arr)
    println("Maximum Product: $maxProduct, Pair: ${pair.first}, ${pair.second}")
}
```

#### Complexity Analysis

  - Time Complexity: O(N²) → We check every pair.
  - Space Complexity: O(1) → No extra space used.

### Approach 2: Sorting (O(N log N))
### Thought Process:

  - Sort the array.
  - The two largest numbers will give one possible max product.
  - The two smallest negative numbers (if exist) might give a larger product.
  - Compare both cases and return the max product.

```
fun maxProductSorting(arr: IntArray): Pair<Int, Pair<Int, Int>> {
    arr.sort()

    val n = arr.size
    val product1 = arr[n - 1] * arr[n - 2]  // Two largest numbers
    val product2 = arr[0] * arr[1]          // Two smallest (negative) numbers

    return if (product1 > product2) {
        Pair(product1, Pair(arr[n - 1], arr[n - 2]))
    } else {
        Pair(product2, Pair(arr[0], arr[1]))
    }
}

// Testing
fun main() {
    val arr = intArrayOf(4, 6, 8, -5, -4, 4)
    val (maxProduct, pair) = maxProductSorting(arr)
    println("Maximum Product: $maxProduct, Pair: ${pair.first}, ${pair.second}")
}

```

#### Complexity Analysis

  - Time Complexity: O(N log N) (due to sorting).
  - Space Complexity: O(1) (no extra space).


### Approach 3: Optimal O(N) - Finding Max and Min in One Pass
#### Thought Process:

  - Scan the array once to find:
        - Top two largest numbers (max1, max2).
        - Smallest two numbers (min1, min2).
  - Compute the two possible products:
       - max1 * max2
    
       - min1 * min2
  - Return the larger product and the corresponding numbers.

```
fun maxProductOptimal(arr: IntArray): Pair<Int, Pair<Int, Int>> {
    if (arr.size < 2) throw IllegalArgumentException("Array must have at least two elements")

    var max1 = Int.MIN_VALUE
    var max2 = Int.MIN_VALUE
    var min1 = Int.MAX_VALUE
    var min2 = Int.MAX_VALUE

    for (num in arr) {
        // Update max values
        when {
            num > max1 -> {
                max2 = max1
                max1 = num
            }
            num > max2 -> {
                max2 = num
            }
        }

        // Update min values
        when {
            num < min1 -> {
                min2 = min1
                min1 = num
            }
            num < min2 -> {
                min2 = num
            }
        }
    }

    val product1 = max1 * max2
    val product2 = min1 * min2

    return if (product1 > product2) {
        Pair(product1, Pair(max1, max2))
    } else {
        Pair(product2, Pair(min1, min2))
    }
}

// Testing
fun main() {
    val arr = intArrayOf(4, 6, 8, -5, -4, 4)
    val (maxProduct, pair) = maxProductOptimal(arr)
    println("Maximum Product: $maxProduct, Pair: ${pair.first}, ${pair.second}")
}
```

### Why is This the Best Approach?

- Step 1: Scan once (O(N)) → Find top 2 largest and smallest.
- Step 2: Compare two products → Get the maximum.
- Step 3: Return the answer → Done in one pass!
