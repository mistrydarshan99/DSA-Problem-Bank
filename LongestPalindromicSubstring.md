LeetCode: https://leetcode.com/problems/longest-palindromic-substring/description/

Given a string s, your task is to find the longest palindromic substring within s.

    A substring is a contiguous sequence of characters within a string, defined as s[i...j] where 0 ≤ i ≤ j < len(s).

    A palindrome is a string that reads the same forward and backward. More formally, s is a palindrome if reverse(s) == s.

Note: If there are multiple palindromic substrings with the same length, return the first occurrence of the longest palindromic substring from left to right.

Examples :

Input: s = “forgeeksskeegfor” 
Output: “geeksskeeg”
Explanation: There are several possible palindromic substrings like “kssk”, “ss”, “eeksskee” etc. But the substring “geeksskeeg” is the longest among all.

Input: s = “Geeks” 
Output: “ee”
Explanation: "ee" is the longest palindromic substring of "Geeks". 

Input: s = “abc” 
Output: “a”
Explanation: "a", "b" and "c" are longest palindromic substrings of same length. So, the first occurrence is returned.

Input: s = “aaa” 
Output: “aaa”

Constraints:
1 ≤ s.size() ≤ 103
s consist of only lowercase English letters.

Let's solve "Longest Palindromic Substring" step by step, starting from brute force (O(N³)) to optimal (O(N)), explaining each approach in an intuitive way.

### Step 1: Understanding the Problem

Given a string s, we need to find the longest contiguous substring that is also a palindrome.
### Key Observations:

  - Single characters are palindromes (e.g., "a", "b", "c").
  - Even-length palindromes exist (e.g., "abba", "geeksskeeg").
  - Odd-length palindromes exist (e.g., "racecar", "aba").
  - A brute-force approach checks all substrings, but we need something more efficient.

### Approach 1: Brute Force (O(N³)) – Checking All Substrings
#### Thought Process:

  - Generate all substrings (i, j).
  - Check if the substring is a palindrome (using two-pointer comparison).
  - Track the longest palindrome found.

```
fun longestPalindromeBruteForce(s: String): String {
    if (s.isEmpty()) return ""

    var maxLength = 0
    var longestPalindrome = ""

    for (i in s.indices) {
        for (j in i until s.length) {
            val substring = s.substring(i, j + 1)

            if (isPalindrome(substring) && substring.length > maxLength) {
                maxLength = substring.length
                longestPalindrome = substring
            }
        }
    }

    return longestPalindrome
}

fun isPalindrome(str: String): Boolean {
    var left = 0
    var right = str.length - 1

    while (left < right) {
        if (str[left] != str[right]) return false
        left++
        right--
    }
    return true
}

// Testing
fun main() {
    val s = "forgeeksskeegfor"
    println("Longest Palindromic Substring: ${longestPalindromeBruteForce(s)}")
}

```

### Complexity Analysis
Time Complexity: Brute Force	O(N³)

Space Complexity: O(1)

### Approach 2: Expand Around Center (O(N²), space-efficient)

#### Thought Process:

  - A palindrome expands symmetrically around its center.
  - For each character in the string, expand outward.
  - Check both odd-length (aba) and even-length (abba) cases.
  - Keep track of the longest palindrome found.

```
fun longestPalindrome(s: String): String {
    var start = 0
    var maxL = 0

    for (i in s.indices) {
        // Check for odd-length palindromes (single center)
        var l = i
        var r = i
        while (l >= 0 && r < s.length && s[l] == s[r]) {
            if (r - l + 1 > maxL) {
                start = l
                maxL = r - l + 1
            }
            l--
            r++
        }

        // Check for even-length palindromes (two centers)
        l = i
        r = i + 1
        while (l >= 0 && r < s.length && s[l] == s[r]) {
            if (r - l + 1 > maxL) {
                start = l
                maxL = r - l + 1
            }
            l--
            r++
        }
    }

    return s.substring(start, start + maxL)
}
```

#### Optmize while Loop as same code is repeated inside while loop

```
fun longestPalindromeOptimizeWhileLoop(s: String): String {
    var start = 0
    var maxL = 0

    fun expandAroundCenter(left: Int, right: Int) {
        var l = left
        var r = right

        while (l >= 0 && r < s.length && s[l] == s[r]) {
            if (r - l + 1 > maxL) {
                start = l
                maxL = r - l + 1
            }
            l--
            r++
        }
    }

    for (i in s.indices) {
        expandAroundCenter(i, i)
        expandAroundCenter(i, i+1)
    }

    return s.substring(start, start + maxL)
}
```

### Complexity Analysis
Time Complexity: Expand Around Center	O(N²)		

Space Complexity: O(1)
