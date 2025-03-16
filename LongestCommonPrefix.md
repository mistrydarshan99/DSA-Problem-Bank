### Problem:

Given an array of strings, find the longest common prefix (LCP) among all the strings.

If there is no common prefix, return an empty string "".
```
Examples:

Input: ["flower","flow","flight"]
Output: "fl"

Input: ["dog","racecar","car"]
Output: ""

Input: ["interspecies","interstellar","interstate"]
Output: "inters"
```

### Approach 1: Brute Force (Character-by-Character Comparison)/Vertical Scanning

We will compare character by character for all strings.
Step-by-step Intuition:

    Take the first string as the base.
    Compare every character in each string column-wise.
    Stop if there is a mismatch or we reach the end of any string.

```
fun longestCommonPrefixBruteForce(strs: Array<String>): String {
    if (strs.isEmpty()) return ""

    var prefix = ""

    for (i in strs[0].indices) { // index loop
        val char = strs[0][i]
        for (j in 1 until strs.size) { // loop for every string
            if (i >= strs[j].length || strs[j][i] != char) {
                return prefix
            }
        }
        prefix += char
    }
    return prefix
}
```
### Time & Space Complexity:
```
    Time: O(n * m) where n = number of strings, m = length of the smallest string.
    Space: O(1) (ignoring the output string).
```

### Approach 2: Horizontal Scanning
Step-by-step Intuition:

  - Take the first string as a prefix.
  - Reduce the prefix by comparing with each string:
    - If prefix is "flow" and next string is "flight", reduce prefix to "fl".

```
fun longestCommonPrefixHorizontal(strs: Array<String>): String {
    if (strs.isEmpty()) return ""

    var prefix = strs[0]
    for (i in 1 until strs.size) {
        while (!strs[i].startsWith(prefix)) {
            prefix = prefix.dropLast(1)
            if (prefix.isEmpty()) return ""
        }
    }
    return prefix
}

```
### Time & Space Complexity:
```
    Time: O(n * m)
    Space: O(1)
```
