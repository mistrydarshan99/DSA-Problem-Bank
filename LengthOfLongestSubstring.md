## Longest Substring Without Repeating Characters

Given a string s, find the length of the longest

without duplicate characters.

 

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

 

Constraints:

    0 <= s.length <= 5 * 104
    s consists of English letters, digits, symbols and spaces.

### 1. Brute Force (O(N2))

#### Let's break it down into a step-by-step intuitive approach where we:

- Check for a repeating character.
- If a repeat is found, decide how to handle it.
- Keep track of the longest substring found so far.

#### Step-by-Step Thought Process

- Start from every index i and grow a substring.
- Use a HashSet to track seen characters.
- As we add characters to the substring:

   - If a repeating character appears, stop and record the length.
   - Move to the next starting index i+1 and repeat.

- Keep track of the longest valid substring encountered.
 
```
fun lengthOfLongestSubstringIntuitive(s: String): Int {
    var maxLength = 0
    var longestSubstring = ""

    for (i in s.indices) { 
        val uniqueChars = mutableSetOf<Char>()
        val currentSubstring = StringBuilder()

        for (j in i until s.length) {
            val currentChar = s[j]

            // Step 1: Identify if current char is a repeat
            if (currentChar in uniqueChars) {
                // Step 2: If a repeat is found, stop expanding and update maxLength
                break
            }

            // Step 3: Otherwise, add it to the substring and continue
            uniqueChars.add(currentChar)
            currentSubstring.append(currentChar)

            // Step 4: Update the longest substring if this one is longer
            if (currentSubstring.length > maxLength) {
                maxLength = currentSubstring.length
                longestSubstring = currentSubstring.toString()
            }
        }
    }

    println("Longest Substring Without Repeating Characters: $longestSubstring")
    return maxLength
}

```
#### Why is This More Intuitive?

- Step 1: Detect a repeating character.
- Step 2: If found, stop expanding the substring.
- Step 3: Otherwise, grow the substring.
- Step 4: Keep track of the longest substring seen so far.

#### Complexity Analysis
Time Complexity	O(N²): Checking Duplicates with HashSet	O(N²)	

Space Complexity: O(N)

##  let's move to the optimal O(N) solution using the Sliding Window technique and explain it step by step in an intuitive way.

### Step-by-Step Thought Process

- Use a sliding window with two pointers (left and right).
- Expand the window (right pointer moves forward) and add characters to a HashMap.
- If a character repeats:

   - Identify the duplicate character.
   - Move the left pointer forward past the previous occurrence of the duplicate.

- Keep track of the longest substring found so far.

```
fun lengthOfLongestSubstringOptimal(s: String): Int {
    val charIndexMap = mutableMapOf<Char, Int>() // Stores last index of each character
    var left = 0 // Left pointer for the window
    var maxLength = 0
    var longestSubstring = ""

    for (right in s.indices) {
        val currentChar = s[right]

        // Step 1: Identify if the current character is a repeat
        if (charIndexMap.containsKey(currentChar)) {
            // Step 2: If a repeat is found, move 'left' past the previous occurrence
            left = maxOf(left, charIndexMap[currentChar]!! + 1)
        }

        // Step 3: Update the last seen index of the current character
        charIndexMap[currentChar] = right

        // Step 4: Calculate the current window size
        val currentLength = right - left + 1
        if (currentLength > maxLength) {
            maxLength = currentLength
            longestSubstring = s.substring(left, right + 1)
        }
    }

    println("Longest Substring Without Repeating Characters: $longestSubstring")
    return maxLength
}

```

### Why is This More Intuitive?

- Step 1: Detect a repeating character using a HashMap.
- Step 2: If found, move left to skip past the previous occurrence.
- Step 3: Update the HashMap with the current character's index.
- Step 4: Keep track of the longest substring while expanding the window.

### Complexity Analysis
Time Complexity: O(N) Sliding Window 

Space Complexity: O(N)

