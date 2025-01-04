### Tries

### 1. Implement Trie (Prefix Tree)

#### Problem Description:
A Trie (Prefix Tree) is a tree-like data structure that stores strings, where each node represents a character in a string. It allows for fast retrieval of words with common prefixes.

#### Operations:
- **Insert**: Insert a word into the Trie.
- **Search**: Search for a word in the Trie.
- **StartsWith**: Check if there is any word in the Trie that starts with a given prefix.

---

#### Golang Solution:
```go
package main

import "fmt"

type TrieNode struct {
	children map[rune]*TrieNode
	isEnd    bool
}

type Trie struct {
	root *TrieNode
}

func Constructor() Trie {
	return Trie{root: &TrieNode{children: make(map[rune]*TrieNode)}}
}

func (this *Trie) Insert(word string) {
	node := this.root
	for _, ch := range word {
		if _, exists := node.children[ch]; !exists {
			node.children[ch] = &TrieNode{children: make(map[rune]*TrieNode)}
		}
		node = node.children[ch]
	}
	node.isEnd = true
}

func (this *Trie) Search(word string) bool {
	node := this.root
	for _, ch := range word {
		if _, exists := node.children[ch]; !exists {
			return false
		}
		node = node.children[ch]
	}
	return node.isEnd
}

func (this *Trie) StartsWith(prefix string) bool {
	node := this.root
	for _, ch := range prefix {
		if _, exists := node.children[ch]; !exists {
			return false
		}
		node = node.children[ch]
	}
	return true
}

func main() {
	trie := Constructor()
	trie.Insert("apple")
	fmt.Println(trie.Search("apple"))  // Output: true
	fmt.Println(trie.Search("app"))    // Output: false
	fmt.Println(trie.StartsWith("app")) // Output: true
}
```

---

#### Python Solution:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.isEnd = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.isEnd = True

    def search(self, word: str) -> bool:
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.isEnd

    def startsWith(self, prefix: str) -> bool:
        node = self.root
        for ch in prefix:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return True

# Sample usage
trie = Trie()
trie.insert("apple")
print(trie.search("apple"))  # Output: True
print(trie.search("app"))    # Output: False
print(trie.startsWith("app")) # Output: True
```

---

### 2. Design Add and Search Words Data Structure

#### Problem Description:
Design a data structure that supports:
- **addWord(word)**: Adds a word to the data structure.
- **search(word)**: Returns `true` if the word is in the data structure. A word could contain dots `.` where any letter could go.

---

#### Golang Solution:
```go
package main

import "fmt"

type WordDictionary struct {
	children map[rune]*WordDictionary
	isEnd    bool
}

func Constructor() WordDictionary {
	return WordDictionary{children: make(map[rune]*WordDictionary)}
}

func (this *WordDictionary) AddWord(word string) {
	node := this
	for _, ch := range word {
		if _, exists := node.children[ch]; !exists {
			node.children[ch] = &WordDictionary{children: make(map[rune]*WordDictionary)}
		}
		node = node.children[ch]
	}
	node.isEnd = true
}

func (this *WordDictionary) Search(word string) bool {
	var searchHelper func(word string, node *WordDictionary) bool
	searchHelper = func(word string, node *WordDictionary) bool {
		if len(word) == 0 {
			return node.isEnd
		}
		ch := rune(word[0])
		if ch == '.' {
			for _, child := range node.children {
				if searchHelper(word[1:], child) {
					return true
				}
			}
			return false
		} else {
			if child, exists := node.children[ch]; exists {
				return searchHelper(word[1:], child)
			}
			return false
		}
	}
	return searchHelper(word, this)
}

func main() {
	dictionary := Constructor()
	dictionary.AddWord("bad")
	dictionary.AddWord("dad")
	dictionary.AddWord("mad")
	fmt.Println(dictionary.Search("pad"))  // Output: false
	fmt.Println(dictionary.Search("bad"))  // Output: true
	fmt.Println(dictionary.Search(".ad"))  // Output: true
	fmt.Println(dictionary.Search("b.."))  // Output: true
}
```

---

#### Python Solution:
```python
class WordDictionary:
    def __init__(self):
        self.children = {}
        self.isEnd = False

    def addWord(self, word: str) -> None:
        node = self
        for ch in word:
            if ch not in node.children:
                node.children[ch] = WordDictionary()
            node = node.children[ch]
        node.isEnd = True

    def search(self, word: str) -> bool:
        def searchHelper(word, node):
            if not word:
                return node.isEnd
            ch = word[0]
            if ch == '.':
                for child in node.children.values():
                    if searchHelper(word[1:], child):
                        return True
                return False
            else:
                if ch in node.children:
                    return searchHelper(word[1:], node.children[ch])
                return False
        return searchHelper(word, self)

# Sample usage
dictionary = WordDictionary()
dictionary.addWord("bad")
dictionary.addWord("dad")
dictionary.addWord("mad")
print(dictionary.search("pad"))  # Output: False
print(dictionary.search("bad"))  # Output: True
print(dictionary.search(".ad"))  # Output: True
print(dictionary.search("b.."))  # Output: True
```

---

### 3. Maximum XOR of Two Numbers in an Array

#### Problem Description:
Given an integer array `nums`, find the maximum value of `nums[i] XOR nums[j]`, where `0 <= i < j < nums.length`.

---

#### Golang Solution:
```go
package main

import "fmt"

func findMaximumXOR(nums []int) int {
	maxXOR := 0
	mask := 0
	for i := 31; i >= 0; i-- {
		mask |= 1 << i
		seen := make(map[int]bool)
		for _, num := range nums {
			seen[num & mask] = true
		}
		possibleMaxXOR := maxXOR | (1 << i)
		for prefix := range seen {
			if seen[prefix^possibleMaxXOR] {
				maxXOR = possibleMaxXOR
				break
			}
		}
	}
	return maxXOR
}

func main() {
	nums := []int{3, 10, 5, 25, 2, 8}
	fmt.Println(findMaximumXOR(nums)) // Output: 28
}
```

---

#### Python Solution:
```python
def findMaximumXOR(nums):
    maxXOR = 0
    mask = 0
    for i in range(31, -1, -1):
        mask |= (1 << i)
        seen = set()
        for num in nums:
            seen.add(num & mask)
        possibleMaxXOR = maxXOR | (1 << i)
        for prefix in seen:
            if possibleMaxXOR ^ prefix in seen:
                maxXOR = possibleMaxXOR
                break
    return maxXOR

# Sample usage
nums = [3, 10, 5, 25, 2, 8]
print(findMaximumXOR(nums))  # Output: 28
```

---

### 4. Word Break

#### Problem Description:
Given a string `s` and a list of strings `wordDict`, determine if `s` can be segmented into a space-separated sequence of one or more dictionary words.

---

#### Golang Solution:
```go
package main

import "fmt"

func wordBreak(s string, wordDict []string) bool {
	wordSet := make(map[string]bool)
	for _, word := range wordDict {
		wordSet[word] = true
	}
	dp := make([]bool, len(s)+1)
	dp[0] = true
	for i := 1; i <= len(s); i++ {
		for j := 0; j < i; j++ {
			if dp[j] && wordSet[s[j:i]] {
				dp[i] = true
				break
			}
		}
	}
	return dp[len(s)]
}

func main() {
	s := "leetcode"
	wordDict := []string{"leet", "code"}
	fmt.Println(wordBreak(s, wordDict)) // Output: true
}
```

---

#### Python Solution:
```python
def wordBreak(s: str, wordDict: list) -> bool:
    wordSet = set(wordDict)
    dp = [False] * (len(s) + 

1)
    dp[0] = True
    for i in range(1, len(s) + 1):
        for j in range(i):
            if dp[j] and s[j:i] in wordSet:
                dp[i] = True
                break
    return dp[len(s)]

# Sample usage
s = "leetcode"
wordDict = ["leet", "code"]
print(wordBreak(s, wordDict))  # Output: True
```

---

### 5. Word Search II

#### Problem Description:
Given a 2D board and a list of words, find all words in the board.

---

#### Golang Solution:
```go
package main

import "fmt"

func findWords(board [][]byte, words []string) []string {
	var result []string
	trie := Constructor()
	for _, word := range words {
		trie.Insert(word)
	}
	
	// DFS function for backtracking
	var dfs func(int, int, *TrieNode, string)
	dfs = func(i, j int, node *TrieNode, word string) {
		if node.isEnd {
			result = append(result, word)
			node.isEnd = false // avoid duplicates
		}
		if i < 0 || i >= len(board) || j < 0 || j >= len(board[0]) || board[i][j] == '#' {
			return
		}
		char := board[i][j]
		child, exists := node.children[char]
		if !exists {
			return
		}
		board[i][j] = '#'
		dfs(i+1, j, child, word+string(char))
		dfs(i-1, j, child, word+string(char))
		dfs(i, j+1, child, word+string(char))
		dfs(i, j-1, child, word+string(char))
		board[i][j] = char
	}
	
	for i := range board {
		for j := range board[0] {
			dfs(i, j, trie.root, "")
		}
	}
	return result
}

func main() {
	board := [][]byte{
		{'o', 'a', 'a', 'n'},
		{'e', 't', 'a', 'e'},
		{'i', 'h', 'k', 'r'},
		{'i', 'f', 'l', 'v'},
	}
	words := []string{"oath", "pea", "eat", "rain"}
	fmt.Println(findWords(board, words))  // Output: ["eat", "oath"]
}
```

---

#### Python Solution:
```python
def findWords(board, words):
    result = []
    trie = Trie()
    for word in words:
        trie.insert(word)
    
    def dfs(i, j, node, word):
        if node.isEnd:
            result.append(word)
            node.isEnd = False  # avoid duplicates
        if i < 0 or i >= len(board) or j < 0 or j >= len(board[0]) or board[i][j] == '#':
            return
        char = board[i][j]
        child, exists = node.children.get(char, None)
        if not exists:
            return
        board[i][j] = '#'
        dfs(i + 1, j, child, word + char)
        dfs(i - 1, j, child, word + char)
        dfs(i, j + 1, child, word + char)
        dfs(i, j - 1, child, word + char)
        board[i][j] = char

    for i in range(len(board)):
        for j in range(len(board[0])):
            dfs(i, j, trie.root, "")
    return result

# Sample usage
board = [
    ['o', 'a', 'a', 'n'],
    ['e', 't', 'a', 'e'],
    ['i', 'h', 'k', 'r'],
    ['i', 'f', 'l', 'v']
]
words = ["oath", "pea", "eat", "rain"]
print(findWords(board, words))  # Output: ["eat", "oath"]
```
