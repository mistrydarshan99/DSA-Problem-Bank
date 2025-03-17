### LeetCode 
https://leetcode.com/problems/majority-element/description/

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

 

Example 1:

Input: nums = [3,2,3]

Output: 3

Example 2:

Input: nums = [2,2,1,1,1,2,2]

Output: 2

 

Constraints:
```
    n == nums.length
    1 <= n <= 5 * 104
    -109 <= nums[i] <= 109
```
 
Follow-up: Could you solve the problem in linear time and in O(1) space?

### Approach 1: Brute Force (O(n²))
Intuition:
- For each element, count its occurrences by iterating the array again.

```
fun majorityElementBrute(nums: IntArray): Int? {
    val n = nums.size
    for (i in nums.indices) {
        var count = 0
        for (j in nums.indices) {
            if (nums[j] == nums[i]) count++
        }
        if (count > n / 2) {
            return nums[i]
        }
    }
    return null // if no majority
}
```

Time Complexity: O(n²)

Space Complexity: O(1)

### Approach 2: Using HashMap (O(n) Time, O(n) Space)

Intuition:

  - Count occurrences of each element using a map.

```
fun majorityElementMap(nums: IntArray): Int? {
    val freqMap = mutableMapOf<Int, Int>()
    val n = nums.size
    for (num in nums) {
        freqMap[num] = freqMap.getOrDefault(num, 0) + 1
        if (freqMap[num]!! > n / 2) return num
    }
    return null
}

```

Time Complexity: O(n)

Space Complexity: O(n)

### Approach 3: Moore's Voting Algorithm (Optimal O(n) Time, O(1) Space)

Intuition:

  - Majority element will cancel out minority elements. We'll do it in two passes.

```
fun majorityElementMoore(nums: IntArray): Int? {
    var count = 0
    var candidate: Int? = null

    // First Pass: Find candidate
    for (num in nums) {
        if (count == 0) {
            candidate = num
        }
        count += if (num == candidate) 1 else -1
    }

    // Second Pass: Validate candidate
    count = 0
    for (num in nums) {
        if (num == candidate) count++
    }
    return if (count > nums.size / 2) candidate else null
}

```

Time Complexity: O(n)

Space Complexity: O(1)

### Step-by-step explanation of Moore's Algorithm

  - First Pass: Treat majority element as "votes". Cancel out every mismatch.
  - Second Pass: Verify that the candidate occurs more than n/2 times.

Why does Moore's work intuitively?

  - Imagine every different number "cancels" out one vote from the majority.
  - The element with enough surplus (majority) will survive this canceling process.

### can we do it in one loop ?

No, Moore's Voting Algorithm requires two passes because:

  - First pass: To find the candidate.
  - Second pass: To validate if the candidate actually occurs more than n/2 times.

#### Why not one pass?

  - In the first pass, we cannot guarantee that the selected candidate is truly the majority just from canceling out.
  - Without checking actual frequency in the second pass, we might return a candidate that does not satisfy the > n/2 condition.

### Alternative: If guaranteed majority exists

If question guarantees there is always a majority element, you can skip the second pass:

```
fun majorityElementGuaranteed(nums: IntArray): Int {
    var candidate = nums[0]
    var count = 0

    for (num in nums) {
        if (count == 0) {
            candidate = num
            count = 1
        } else if (num == candidate) {
            count++
        } else {
            count--
        }
    }
    return candidate // No need for second pass
}
```

### But ⚠️: If NOT guaranteed → 2 passes are needed.
