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
