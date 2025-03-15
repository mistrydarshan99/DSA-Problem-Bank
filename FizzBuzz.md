Given an integer n, return a string array answer (1-indexed) where:

    answer[i] == "FizzBuzz" if i is divisible by 3 and 5.
    answer[i] == "Fizz" if i is divisible by 3.
    answer[i] == "Buzz" if i is divisible by 5.
    answer[i] == i (as a string) if none of the above conditions are true.

 

Example 1:

Input: n = 3
Output: ["1","2","Fizz"]

Example 2:

Input: n = 5
Output: ["1","2","Fizz","4","Buzz"]

Example 3:

Input: n = 15
Output: ["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"]


### Approach 1: Brute Force (O(n))

We iterate through numbers from 1 to n, checking divisibility conditions.

```
fun fizzBuzz(n: Int): List<String> {
    val result = mutableListOf<String>()
    
    for (i in 1..n) {
        when {
            i % 3 == 0 && i % 5 == 0 -> result.add("FizzBuzz")
            i % 3 == 0 -> result.add("Fizz")
            i % 5 == 0 -> result.add("Buzz")
            else -> result.add(i.toString())
        }
    }
    
    return result
}

// Testing
fun main() {
    println(fizzBuzz(15))
}

```

#### Time & Space Complexity
Time Complexity: Brute Force (Loop)	O(n)

Space Complexity: O(n)

### Approach 2: String Concatenation Optimization (O(n))

Instead of checking for each condition separately, we build the string dynamically.

```
fun fizzBuzzOptimized(n: Int): List<String> {
    val result = mutableListOf<String>()

    for (i in 1..n) {
        var str = ""

        if (i % 3 == 0) str += "Fizz"
        if (i % 5 == 0) str += "Buzz"
        if (str.isEmpty()) str = i.toString()

        result.add(str)
    }
    return result
}

// Testing
fun main() {
    println(fizzBuzzOptimized(15))
}
```

### Why is this better?

- Removes redundant conditions (i % 3 == 0 && i % 5 == 0).
- Avoids unnecessary comparisons (more readable).
- Still O(n) time complexity but fewer operations per iteration.
