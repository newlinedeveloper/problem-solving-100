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
```
# reverse_string takes a string as input and returns the reversed string
def reverse_string(s):
    # Convert the string to a list (to handle character manipulation)
    chars = list(s)
    
    # Use two pointers to swap the characters in place
    i, j = 0, len(chars) - 1
    while i < j:
        chars[i], chars[j] = chars[j], chars[i]
        i += 1
        j -= 1
    
    # Convert the list back to a string and return it
    return ''.join(chars)

# main is the entry point for the program
if __name__ == "__main__":
    input_string = "hello"
    reversed_string = reverse_string(input_string)
    print("Original:", input_string)
    print("Reversed:", reversed_string)

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

```
package main

import (
	"fmt"
	"sort"
	"strings"
)

func groupAnagrams(strs []string) [][]string {
	// Hash map to group anagrams
	anagrams := make(map[string][]string)

	for _, str := range strs {
		// Sort the string to use as a key
		key := sortString(str)
		anagrams[key] = append(anagrams[key], str)
	}

	// Collect the grouped anagrams
	result := [][]string{}
	for _, group := range anagrams {
		result = append(result, group)
	}

	return result
}

// Helper function to sort a string
func sortString(s string) string {
	chars := strings.Split(s, "")
	sort.Strings(chars)
	return strings.Join(chars, "")
}

func main() {
	// Test cases
	input := []string{"eat", "tea", "tan", "ate", "nat", "bat"}
	fmt.Println(groupAnagrams(input)) // Output: [["eat" "tea" "ate"] ["tan" "nat"] ["bat"]]

	input2 := []string{"a"}
	fmt.Println(groupAnagrams(input2)) // Output: [["a"]]

	input3 := []string{"", ""}
	fmt.Println(groupAnagrams(input3)) // Output: [["" ""]]
}


```


### Roman to Integer

```
package main

import (
    "fmt"
)

func romanToInt(s string) int {
    // Map to store Roman numerals and their corresponding values
    romanMap := map[byte]int{
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000,
    }

    // Initialize total and length of string
    total := 0
    n := len(s)

    for i := 0; i < n; i++ {
        // Check if the current character is less than the next character
        if i < n-1 && romanMap[s[i]] < romanMap[s[i+1]] {
            total -= romanMap[s[i]]
        } else {
            total += romanMap[s[i]]
        }
    }

    return total
}

func main() {
    // Test cases
    fmt.Println(romanToInt("III"))    // Output: 3
    fmt.Println(romanToInt("IV"))     // Output: 4
    fmt.Println(romanToInt("IX"))     // Output: 9
    fmt.Println(romanToInt("LVIII"))  // Output: 58
    fmt.Println(romanToInt("MCMXCIV")) // Output: 1994
}


```

### Problem 1: Sort Characters By Frequency

#### Problem Description:
Given a string `s`, sort it in decreasing order based on the frequency of the characters. If two characters have the same frequency, their order in the output does not matter.

---

#### Pseudocode:
1. Create a frequency map to count occurrences of each character.
2. Convert the map into a list of tuples and sort by frequency in descending order.
3. Construct the result string by repeating each character based on its frequency.
4. Return the result.

---

#### Golang Solution:

```go
package main

import (
	"fmt"
	"sort"
)

func frequencySort(s string) string {
	// Frequency map
	freq := make(map[rune]int)
	for _, ch := range s {
		freq[ch]++
	}

	// Create a slice of characters
	type charFreq struct {
		char  rune
		count int
	}
	charFreqs := make([]charFreq, 0, len(freq))
	for ch, count := range freq {
		charFreqs = append(charFreqs, charFreq{ch, count})
	}

	// Sort by frequency in descending order
	sort.Slice(charFreqs, func(i, j int) bool {
		return charFreqs[i].count > charFreqs[j].count
	})

	// Build the result
	result := ""
	for _, cf := range charFreqs {
		result += string(cf.char) * cf.count
	}
	return result
}

func main() {
	fmt.Println(frequencySort("tree"))       // Output: "eert"
	fmt.Println(frequencySort("cccaaa"))    // Output: "cccaaa" or "aaaccc"
	fmt.Println(frequencySort("Aabb"))      // Output: "bbAa"
}
```

---

#### Python Solution:

```python
from collections import Counter

def frequencySort(s: str) -> str:
    # Frequency map
    freq = Counter(s)
    
    # Sort characters by frequency in descending order
    sorted_chars = sorted(freq.items(), key=lambda x: -x[1])
    
    # Build the result string
    result = ''.join(char * count for char, count in sorted_chars)
    return result

# Test cases
print(frequencySort("tree"))    # Output: "eert"
print(frequencySort("cccaaa"))  # Output: "cccaaa" or "aaaccc"
print(frequencySort("Aabb"))    # Output: "bbAa"
```

---

### Problem 2: Minimum Remove to Make Valid Parentheses

#### Problem Description:
Given a string `s` containing only `(`, `)`, and lowercase English characters, remove the minimum number of parentheses to make the string valid.

---

#### Pseudocode:
1. Use a stack to track indices of unmatched `(`.
2. Traverse the string:
   - If `(`, push its index onto the stack.
   - If `)`, pop the stack if it's not empty; otherwise, mark it for removal.
3. Combine the stack with other marked indices.
4. Construct the result string excluding marked indices.

---

#### Golang Solution:

```go
package main

import (
	"fmt"
)

func minRemoveToMakeValid(s string) string {
	stack := []int{}
	toRemove := make(map[int]bool)

	// Traverse to find unmatched parentheses
	for i, ch := range s {
		if ch == '(' {
			stack = append(stack, i)
		} else if ch == ')' {
			if len(stack) > 0 {
				stack = stack[:len(stack)-1]
			} else {
				toRemove[i] = true
			}
		}
	}

	// Mark remaining '(' in the stack
	for _, index := range stack {
		toRemove[index] = true
	}

	// Build the result string
	result := ""
	for i, ch := range s {
		if !toRemove[i] {
			result += string(ch)
		}
	}
	return result
}

func main() {
	fmt.Println(minRemoveToMakeValid("lee(t(c)o)de)")) // Output: "lee(t(c)o)de"
	fmt.Println(minRemoveToMakeValid("a)b(c)d"))       // Output: "ab(c)d"
	fmt.Println(minRemoveToMakeValid("))(("))          // Output: ""
}
```

---

#### Python Solution:

```python
def minRemoveToMakeValid(s: str) -> str:
    stack = []
    to_remove = set()

    # Traverse to find unmatched parentheses
    for i, char in enumerate(s):
        if char == '(':
            stack.append(i)
        elif char == ')':
            if stack:
                stack.pop()
            else:
                to_remove.add(i)

    # Add remaining '(' in the stack to removal set
    to_remove.update(stack)

    # Build the result string
    result = ''.join(char for i, char in enumerate(s) if i not in to_remove)
    return result

# Test cases
print(minRemoveToMakeValid("lee(t(c)o)de)"))  # Output: "lee(t(c)o)de"
print(minRemoveToMakeValid("a)b(c)d"))        # Output: "ab(c)d"
print(minRemoveToMakeValid("))(("))           # Output: ""
```

---

### Problem 3: Permutation in String

#### Problem Description:
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`.

---

#### Pseudocode:
1. Create a frequency map for `s1`.
2. Use a sliding window of length equal to `s1` on `s2`.
3. For each window:
   - Update the frequency map for the characters in the window.
   - Check if the maps match.
4. Return `true` if any window matches the map.

---

#### Golang Solution:

```go
package main

import (
	"fmt"
)

func checkInclusion(s1 string, s2 string) bool {
	if len(s1) > len(s2) {
		return false
	}

	count1 := make(map[byte]int)
	count2 := make(map[byte]int)

	// Initialize count1 for s1
	for i := range s1 {
		count1[s1[i]]++
	}

	// Sliding window
	for i := range s2 {
		count2[s2[i]]++

		// Maintain window size
		if i >= len(s1) {
			count2[s2[i-len(s1)]]--
			if count2[s2[i-len(s1)]] == 0 {
				delete(count2, s2[i-len(s1)])
			}
		}

		// Compare maps
		if len(count1) == len(count2) {
			match := true
			for k, v := range count1 {
				if count2[k] != v {
					match = false
					break
				}
			}
			if match {
				return true
			}
		}
	}
	return false
}

func main() {
	fmt.Println(checkInclusion("ab", "eidbaooo")) // Output: true
	fmt.Println(checkInclusion("ab", "eidboaoo")) // Output: false
}
```

---

#### Python Solution:

```python
from collections import Counter

def checkInclusion(s1: str, s2: str) -> bool:
    if len(s1) > len(s2):
        return False

    count1 = Counter(s1)
    count2 = Counter(s2[:len(s1)])

    # Sliding window
    for i in range(len(s1), len(s2)):
        if count1 == count2:
            return True
        count2[s2[i]] += 1
        count2[s2[i - len(s1)]] -= 1
        if count2[s2[i - len(s1)]] == 0:
            del count2[s2[i - len(s1)]]

    return count1 == count2

# Test cases
print(checkInclusion("ab", "eidbaooo"))  # Output: true
print(checkInclusion("ab", "eidboaoo"))  # Output: false
```

--- 
