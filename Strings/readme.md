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
```

```

### Group Anagrams


### Roman to Integer


### Sort Characters By Frequency


### Minimum Remove to Make Valid Parentheses


### Permutation in String
