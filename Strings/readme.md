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

// isAnagram compares two maps and returns true if they have the same character counts.
func isAnagram(countP, countS map[rune]int) bool {
	for k, v := range countP {
		if countS[k] != v {
			return false
		}
	}
	return true
}

// findAnagrams finds all start indices of p's anagrams in s.
func findAnagrams(s string, p string) []int {
	var results []int
	lenS, lenP := len(s), len(p)

	// Edge case: if p is longer than s, there can't be an anagram.
	if lenS < lenP {
		return results
	}

	countP := make(map[rune]int)
	countS := make(map[rune]int)

	// Initialize the character counts for p and the first window of s.
	for _, ch := range p {
		countP[ch]++
	}
	for _, ch := range s[:lenP] {
		countS[ch]++
	}

	// Slide the window over s one character at a time
	for i := 0; i <= lenS-lenP; i++ {
		// If at the start index the character counts match, it's an anagram.
		if isAnagram(countP, countS) {
			results = append(results, i)
		}

		// Slide the window: remove the leftmost character count and add the next one.
		if i+lenP < lenS {
			countS[rune(s[i])]--
			if countS[rune(s[i])] == 0 {
				delete(countS, rune(s[i]))
			}
			countS[rune(s[i+lenP])]++
		}
	}
	return results
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
