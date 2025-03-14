## Permutations of given String

Examples:

    Input:  s = “ABC”
    Output: “ABC”, “ACB”, “BAC”, “BCA”, “CBA”, “CAB”

    Input: s = “XY”
    Output: “XY”, “YX”

    Input: s = “AAA”
    Output: “AAA”, “AAA”, “AAA”, “AAA”, “AAA”, “AAA” 

### Approach 1: Brute Force (O(N!) Time)

Intuition:

    - Generate all possible orderings of characters by swapping them in all positions.
    - Use backtracking to explore every possible arrangement.

```
fun getPermutations(str: String): List<String> {
    val result = mutableListOf<String>()
    permute(str.toCharArray(), 0, result)
    return result
}

fun permute(chars: CharArray, start: Int, result: MutableList<String>) {
    if (start == chars.size - 1) {
        result.add(String(chars))
        return
    }

    for (i in start until chars.size) {
        swap(chars, start, i)
        permute(chars, start + 1, result)
        swap(chars, start, i) // Backtrack
    }
}

fun swap(arr: CharArray, i: Int, j: Int) {
    val temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}

// Testing
fun main() {
    val s = "ABC"
    println(getPermutations(s))
}

```

#### Time & Space Complexity
Time Complexity: Backtracking	O(N!)	

Space Complexity: O(N!) (Storing all permutations)

## Recursive solution but thinking process is different here 

#### Explanation

- Base Case: If the string has one character or is empty, return a list containing the string itself as the only permutation.
- Recursive Step:
    - For each character in the string:
        - Fix the character at the beginning.
        - Generate permutations of the remaining characters by recursively calling the permutations function.
        - Append the fixed character to each of these permutations and add the result to a set to avoid duplicates.
- Return the Result: Convert the set of permutations to a list and return it.

```
Base Case: If the string is "A", the only permutation is "A".
Recursive Step:
Fix 'A' at the beginning and generate permutations of "BC".
Fix 'B' at the beginning and generate permutations of "C".
Fix 'C' at the beginning and return "C" as the only permutation.
Combine these results to get "BC" and "CB".
Prepend 'A' to these results to get "ABC" and "ACB".
Fix 'B' at the beginning and generate permutations of "AC".
Fix 'A' at the beginning and generate permutations of "C".
Fix 'C' at the beginning and return "C" as the only permutation.
Combine these results to get "AC" and "CA".
Prepend 'B' to these results to get "BAC" and "BCA".
Fix 'C' at the beginning and generate permutations of "AB".
Fix 'A' at the beginning and generate permutations of "B".
Fix 'B' at the beginning and return "B" as the only permutation.
Combine these results to get "AB" and "BA".
Prepend 'C' to these results to get "CBA" and "CAB".
```

```
fun permutations(s: String): List<String> {
    // Base case: if the string is empty or has one character, return it as a single permutation
    if (s.length == 1) {
        return listOf(s)
    }

    // Recursive step: generate permutations
    val perms = mutableSetOf<String>() // Use a set to avoid duplicates
    for (i in s.indices) {
        // Fix the i-th character
        val char = s[i]
        // Generate permutations of the remaining characters
        val remaining = s.removeRange(i, i + 1)
        val remainingResultList = permutations(remaining)
        for (perm in remainingResultList) {
            // Append the fixed character to each permutation of the remaining characters
            perms.add(char + perm)
        }
    }

    return perms.toList()
}
```
