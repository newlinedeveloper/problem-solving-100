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


### Longest Repeating Character Replacement


### Group Anagrams


### Roman to Integer


### Sort Characters By Frequency


### Minimum Remove to Make Valid Parentheses


### Permutation in String
