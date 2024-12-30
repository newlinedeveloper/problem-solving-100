### Dynamic programming

### **1. Maximum Product Subarray**

#### Problem:
Find the contiguous subarray within an array (containing at least one number) that has the largest product.

#### Pseudocode:
1. Use two variables: `maxProduct` and `minProduct` to track the current maximum and minimum products.
2. Iterate through the array, updating the current maximum and minimum products.
3. Keep track of the global maximum product.

#### Solution in Golang:
```go
func maxProduct(nums []int) int {
    maxProduct := nums[0]
    minProduct := nums[0]
    result := nums[0]

    for i := 1; i < len(nums); i++ {
        if nums[i] < 0 {
            maxProduct, minProduct = minProduct, maxProduct
        }
        maxProduct = max(nums[i], maxProduct*nums[i])
        minProduct = min(nums[i], minProduct*nums[i])
        result = max(result, maxProduct)
    }
    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    nums := []int{2, 3, -2, 4}
    fmt.Println(maxProduct(nums)) // Output: 6
}
```

#### Input:
```
nums = [2, 3, -2, 4]
```

#### Output:
```
6
```

---

### **2. Longest Increasing Subsequence**

#### Problem:
Find the length of the longest strictly increasing subsequence in an array.

#### Pseudocode:
1. Use a dynamic programming (DP) array `dp` where `dp[i]` represents the length of the LIS ending at index `i`.
2. Iterate through the array, updating `dp` values.
3. Return the maximum value in `dp`.

#### Solution in Golang:
```go
func lengthOfLIS(nums []int) int {
    dp := make([]int, len(nums))
    for i := range dp {
        dp[i] = 1
    }
    maxLen := 1
    for i := 1; i < len(nums); i++ {
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] {
                dp[i] = max(dp[i], dp[j]+1)
            }
        }
        maxLen = max(maxLen, dp[i])
    }
    return maxLen
}

func main() {
    nums := []int{10, 9, 2, 5, 3, 7, 101, 18}
    fmt.Println(lengthOfLIS(nums)) // Output: 4
}
```

#### Input:
```
nums = [10, 9, 2, 5, 3, 7, 101, 18]
```

#### Output:
```
4
```

---

### **3. Edit Distance**

#### Problem:
Find the minimum number of operations (insertions, deletions, replacements) required to convert one string into another.

#### Pseudocode:
1. Create a DP table where `dp[i][j]` represents the edit distance between the first `i` characters of `word1` and the first `j` characters of `word2`.
2. Populate the DP table using insertion, deletion, and replacement rules.
3. Return `dp[len(word1)][len(word2)]`.

#### Solution in Golang:
```go
func minDistance(word1, word2 string) int {
    m, n := len(word1), len(word2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

    for i := 0; i <= m; i++ {
        for j := 0; j <= n; j++ {
            if i == 0 {
                dp[i][j] = j
            } else if j == 0 {
                dp[i][j] = i
            } else if word1[i-1] == word2[j-1] {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = 1 + min(dp[i-1][j-1], min(dp[i-1][j], dp[i][j-1]))
            }
        }
    }
    return dp[m][n]
}

func main() {
    word1 := "horse"
    word2 := "ros"
    fmt.Println(minDistance(word1, word2)) // Output: 3
}
```

#### Input:
```
word1 = "horse", word2 = "ros"
```

#### Output:
```
3
```

---

### **4. Coin Change**

#### Problem:
Find the minimum number of coins needed to make a given amount using coins of given denominations.

#### Pseudocode:
1. Create a DP array where `dp[i]` represents the minimum coins required to make the amount `i`.
2. For each coin, update the DP array.
3. Return `dp[amount]`.

#### Solution in Golang:
```go
func coinChange(coins []int, amount int) int {
    dp := make([]int, amount+1)
    for i := range dp {
        dp[i] = amount + 1
    }
    dp[0] = 0

    for _, coin := range coins {
        for j := coin; j <= amount; j++ {
            dp[j] = min(dp[j], dp[j-coin]+1)
        }
    }

    if dp[amount] > amount {
        return -1
    }
    return dp[amount]
}

func main() {
    coins := []int{1, 2, 5}
    amount := 11
    fmt.Println(coinChange(coins, amount)) // Output: 3
}
```

#### Input:
```
coins = [1, 2, 5], amount = 11
```

#### Output:
```
3
```

---
### **5. Partition Equal Subset Sum**

#### Problem:
Determine if the array can be partitioned into two subsets with equal sums.

#### Pseudocode:
1. Compute the total sum of the array. If the sum is odd, return `false`.
2. Use a DP array where `dp[i]` indicates whether a subset sum of `i` is possible.
3. Iterate through the array, updating the DP array for possible subset sums.
4. Return `dp[totalSum/2]`.

#### Solution in Golang:
```go
func canPartition(nums []int) bool {
    totalSum := 0
    for _, num := range nums {
        totalSum += num
    }
    if totalSum%2 != 0 {
        return false
    }

    target := totalSum / 2
    dp := make([]bool, target+1)
    dp[0] = true

    for _, num := range nums {
        for j := target; j >= num; j-- {
            dp[j] = dp[j] || dp[j-num]
        }
    }
    return dp[target]
}

func main() {
    nums := []int{1, 5, 11, 5}
    fmt.Println(canPartition(nums)) // Output: true
}
```

#### Input:
```
nums = [1, 5, 11, 5]
```

#### Output:
```
true
```

---

### **6. Longest Common Subsequence**

#### Problem:
Given two strings, find the length of their longest common subsequence.

#### Pseudocode:
1. Create a DP table where `dp[i][j]` represents the LCS of the first `i` characters of `text1` and the first `j` characters of `text2`.
2. Populate the DP table based on whether the current characters match or not.
3. Return `dp[len(text1)][len(text2)]`.

#### Solution in Golang:
```go
func longestCommonSubsequence(text1, text2 string) int {
    m, n := len(text1), len(text2)
    dp := make([][]int, m+1)
    for i := range dp {
        dp[i] = make([]int, n+1)
    }

    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if text1[i-1] == text2[j-1] {
                dp[i][j] = dp[i-1][j-1] + 1
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
            }
        }
    }
    return dp[m][n]
}

func main() {
    text1 := "abcde"
    text2 := "ace"
    fmt.Println(longestCommonSubsequence(text1, text2)) // Output: 3
}
```

#### Input:
```
text1 = "abcde", text2 = "ace"
```

#### Output:
```
3
```

---

### **7. Word Break**

#### Problem:
Given a string and a dictionary of words, determine if the string can be segmented into a space-separated sequence of dictionary words.

#### Pseudocode:
1. Use a DP array where `dp[i]` indicates whether the substring `s[0:i]` can be segmented.
2. Iterate through the string, checking all possible substrings.
3. Return `dp[len(s)]`.

#### Solution in Golang:
```go
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

#### Input:
```
s = "leetcode", wordDict = ["leet", "code"]
```

#### Output:
```
true
```

---

### **8. Unique Paths**

#### Problem:
Find the number of unique paths from the top-left corner to the bottom-right corner of a grid.

#### Pseudocode:
1. Create a DP table where `dp[i][j]` represents the number of ways to reach cell `(i, j)`.
2. Populate the DP table using the rule: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.
3. Return `dp[m-1][n-1]`.

#### Solution in Golang:
```go
func uniquePaths(m int, n int) int {
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
        dp[i][0] = 1
    }
    for j := 0; j < n; j++ {
        dp[0][j] = 1
    }

    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            dp[i][j] = dp[i-1][j] + dp[i][j-1]
        }
    }
    return dp[m-1][n-1]
}

func main() {
    m, n := 3, 7
    fmt.Println(uniquePaths(m, n)) // Output: 28
}
```

#### Input:
```
m = 3, n = 7
```

#### Output:
```
28
```

---
