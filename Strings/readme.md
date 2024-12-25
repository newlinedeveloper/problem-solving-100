## Strings

### Reverse String

```
package main

import (
	"fmt"
)

// reverseString takes a string as input and returns the reversed string
func reverseString(s string) string {
	// Convert the string to a rune slice (to properly handle multi-byte characters)
	runes := []rune(s)
	
	// Use two pointers to swap the characters in place
	for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
		runes[i], runes[j] = runes[j], runes[i]
	}
	
	// Convert the rune slice back to a string and return it
	return string(runes)
}

// main is the entry point for the program
func main() {
	input := "hello"
	reversed := reverseString(input)
	fmt.Println("Original:", input)
	fmt.Println("Reversed:", reversed)
}
```


### Find All Anagrams in a String

```
package main

import (
	"fmt"
)

func findAnagrams(s string, p string) []int {
    result := []int{}
    targetCount := make(map[byte]int)
    windowCount := make(map[byte]int)

    // Populate targetCount with frequency of characters in p
    for i := 0; i < len(p); i++ {
        targetCount[p[i]]++
    }
    
    windowSize := len(p)

    for i := 0; i < len(s); i++ {
        // Add current character to windowCount
        windowCount[s[i]]++
        
        // Remove the leftmost character if window exceeds size
        if i >= windowSize {
            if windowCount[s[i-windowSize]] == 1 {
                delete(windowCount, s[i-windowSize])
            } else {
                windowCount[s[i-windowSize]]--
            }
        }

        // Compare targetCount and windowCount
        if equalMaps(targetCount, windowCount) {
            result = append(result, i-windowSize+1)
        }
    }

    return result
}

// Helper function to compare two maps
func equalMaps(a, b map[byte]int) bool {
    if len(a) != len(b) {
        return false
    }
    for k, v := range a {
        if b[k] != v {
            return false
        }
    }
    return true
}


func main() {
	s := "cbaebabacd"
	p := "abc"
	anagrams := findAnagrams(s, p)
	fmt.Println(anagrams) // Output: [0 6]
}

```


### Minimum Window Substring
```
package main

import (
	"fmt"
	"math"
)

Input: string s, string t
Output: smallest substring in s containing all characters of t

1. Initialize:
   - Frequency map `tCount` for characters in t.
   - A frequency map `windowCount` for the current window in s.
   - Two pointers: `start` and `end` for sliding window.
   - Variables to track minimum window length and its starting index.

2. Expand the window by moving `end`:
   - Add the character at `end` to `windowCount`.
   - Check if the current window contains all characters in t.

3. Once a valid window is found:
   - Try to shrink the window by moving `start`.
   - Update the minimum window length and starting index if a smaller valid window is found.
   - Remove characters from `windowCount` as `start` moves.

4. Repeat steps 2-3 until `end` reaches the end of s.

5. Return the substring based on the minimum window length.


func minWindow(s string, t string) string {
    if len(s) < len(t) {
        return ""
    }

    // Frequency map for characters in t
    tCount := make(map[byte]int)
    for i := 0; i < len(t); i++ {
        tCount[t[i]]++
    }

    // Variables for sliding window
    windowCount := make(map[byte]int)
    required := len(tCount)
    formed := 0

    left, right := 0, 0
    minLength := len(s) + 1
    startIndex := 0

    // Sliding window
    for right < len(s) {
        char := s[right]
        windowCount[char]++

        // Check if current character contributes to a valid window
        if tCount[char] > 0 && windowCount[char] == tCount[char] {
            formed++
        }

        // Try to shrink the window
        for left <= right && formed == required {
            char = s[left]

            // Update the result if this window is smaller
            if right-left+1 < minLength {
                minLength = right - left + 1
                startIndex = left
            }

            // Remove the leftmost character from the window
            windowCount[char]--
            if tCount[char] > 0 && windowCount[char] < tCount[char] {
                formed--
            }
            left++
        }

        right++
    }

    // If no valid window was found
    if minLength > len(s) {
        return ""
    }

    return s[startIndex : startIndex+minLength]
}


func main() {
	s := "ADOBECODEBANC"
	t := "ABC"
	fmt.Println("Minimum window substring:", minWindow(s, t)) // Output: "BANC"
}
```


### Longest Repeating Character Replacement

Given a string `s` and an integer `k`, you are allowed to replace up to `k` characters in the string so that it contains only one distinct character. Return the length of the longest substring containing the same letter you can get after the replacements.

---

### **Approach**

We solve this problem using the **sliding window technique**. The key observation is that the longest substring can be determined by:

1. Keeping track of the count of the most frequent character in the current window.
2. Checking if the remaining characters in the window (i.e., `window_size - max_count`) can be replaced within the limit of `k`.

If the current window is valid, we expand it; otherwise, we shrink it.

---

### **Pseudocode**

```plaintext
Input: string s, integer k
Output: length of the longest substring after replacing at most k characters

1. Initialize:
   - `max_count` to keep track of the most frequent character count in the current window.
   - A frequency map `char_count` for characters in the current window.
   - Two pointers: `start` and `end` for sliding window.
   - `max_length` to store the maximum length of the valid substring.

2. Expand the window by moving `end`:
   - Add the character at `end` to `char_count`.
   - Update `max_count` with the frequency of the most frequent character.

3. Check if the current window is valid:
   - If `(window_size - max_count) > k`, move `start` to shrink the window.

4. Update `max_length` with the size of the current valid window.

5. Repeat steps 2-4 until `end` reaches the end of `s`.

6. Return `max_length`.
```

---

### **Solution in Go**

```go
func characterReplacement(s string, k int) int {
    charCount := make(map[byte]int) // Frequency map
    maxCount := 0                   // Most frequent character count
    start := 0                      // Start of the window
    maxLength := 0                  // Result: maximum length of the valid substring

    for end := 0; end < len(s); end++ {
        char := s[end]
        charCount[char]++
        maxCount = max(maxCount, charCount[char])

        // Check if the current window is valid
        if (end - start + 1 - maxCount) > k {
            charCount[s[start]]--
            start++
        }

        // Update the maxLength
        maxLength = max(maxLength, end-start+1)
    }

    return maxLength
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

---

### **Explanation**

1. **Initialization**:
   - `charCount` keeps track of character frequencies in the current window.
   - `maxCount` stores the count of the most frequent character in the window.

2. **Expand the Window**:
   - For each character in `s`, increment its count in `charCount`.
   - Update `maxCount` with the maximum frequency seen so far.

3. **Check Window Validity**:
   - If the number of characters to replace `(window_size - maxCount)` exceeds `k`, shrink the window by incrementing `start` and updating `charCount`.

4. **Update Result**:
   - The valid window size `(end - start + 1)` is compared with `maxLength`, and the maximum is stored.

5. **Output**:
   - The length of the longest valid substring is returned.

---

### **Test Cases**

#### Test Case 1:
```go
s := "ABAB"
k := 2
fmt.Println(characterReplacement(s, k)) // Output: 4
```
- Replace both `B`s with `A`s or both `A`s with `B`s to get `"AAAA"` or `"BBBB"`.

#### Test Case 2:
```go
s := "AABABBA"
k := 1
fmt.Println(characterReplacement(s, k)) // Output: 4
```
- Replace one `B` to get `"AABAAAA"` or `"AABBBAA"`.

#### Test Case 3:
```go
s := "ABCDE"
k := 1
fmt.Println(characterReplacement(s, k)) // Output: 2
```
- Replace any one character to get a substring of length 2 with repeating characters.

---

### **Complexity Analysis**

- **Time Complexity**: \(O(n)\)
  - The sliding window ensures that each character in `s` is processed at most twice (once when expanding and once when contracting).

- **Space Complexity**: \(O(1)\)
  - The `charCount` map has a fixed size of at most 26 entries (for English alphabet).

---

This solution efficiently calculates the length of the longest valid substring using the sliding window approach.

### Group Anagrams


### Roman to Integer


### Sort Characters By Frequency


### Minimum Remove to Make Valid Parentheses


### Permutation in String
