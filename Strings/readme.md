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

// minWindow finds the minimum window in s which will contain all the characters in t
func minWindow(s string, t string) string {
	if len(s) == 0 || len(t) == 0 {
		return ""
	}

	// Dictionary which keeps a count of all the unique characters in t.
	dictT := make(map[rune]int)
	for _, c := range t {
		dictT[c]++
	}

	// Number of unique characters in t, which need to be present in the desired window.
	required := len(dictT)

	// Left and Right pointer
	l, r := 0, 0

	// Formed is used to keep track of how many unique characters in t
	// are present in the current window in its desired frequency.
	// e.g. if t is "AABC" then the window must have two A's, one B and one C.
	// Thus formed would be = 3 when all these conditions are met.
	formed := 0

	// Dictionary which keeps a count of all the unique characters in the current window.
	windowCounts := make(map[rune]int)

	// ans list of the form (window length, left, right)
	ans := []int{-1, 0, 0}

	for r < len(s) {
		// Add one character from the right to the window
		c := rune(s[r])
		windowCounts[c]++
		// If the frequency of the current character added equals to the
		// desired count in t then increment the formed count by 1.
		if count, ok := dictT[c]; ok && windowCounts[c] == count {
			formed++
		}

		// Try and contract the window till the point where it ceases to be 'desirable'.
		for l <= r && formed == required {
			c = rune(s[l])
			// Save the smallest window until now.
			if ans[0] == -1 || r-l+1 < ans[0] {
				ans[0] = r - l + 1
				ans[1] = l
				ans[2] = r
			}

			// The character at the position pointed by the `Left` pointer is no longer a part of the window.
			windowCounts[c]--
			if count, ok := dictT[c]; ok && windowCounts[c] < count {
				formed--
			}

			// Move the left pointer ahead, this would help to look for a new window.
			l++
		}

		// Keep expanding the window once we are done contracting.
		r++
	}

	if ans[0] == -1 {
		return ""
	}
	return s[ans[1] : ans[2]+1]
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
