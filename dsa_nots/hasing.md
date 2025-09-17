# Hashing Techniques - Complete Guide

## Introduction

Hashing is a technique for storing numbers at their corresponding indices in an array. It provides efficient insertion, deletion, and lookup operations with average time complexity of O(1).

## Basic Concept

In basic hashing, we store a number at its index position in the array. For example, if we have number 5, we store it at index 5.

### Problem with Large Numbers

When the number is greater than the array size, we can't directly use it as an index. Solution: use **modulo operation** - store the number at index `number % array_size`.

However, this leads to **collisions** - different numbers mapping to the same index.

## Collision Resolution Techniques

### 1. Chaining

- Create a linked list at each index
- Append colliding elements to the linked list at that index
- Each index can store multiple values

### 2. Linear Probing

- When collision occurs, move to the next available index
- Formula: `(hash_value + i) % table_size` where i = 1, 2, 3...
- Continue probing until an empty slot is found

### 3. Quadratic Probing

- Similar to linear probing but uses quadratic increments
- Formula: `(hash_value + i²) % table_size` where i = 1, 2, 3...
- Reduces clustering compared to linear probing

## Python Implementation: Dictionary

In Python, hashing is implemented through **dictionaries** which handle all collision resolution internally.

## Practice Problems

### 1. Two Sum

**Problem:** Find indices of two numbers that add up to target.

**Example:**

- Input: nums = [2,7,11,15], target = 9
- Output: [0,1]
- Explanation: nums[0] + nums[1] = 2 + 7 = 9

**Code:**

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
    dic = {}
    for i in range(len(nums)):
        if target - nums[i] in dic:
            return [dic[target - nums[i]], i]
        else:
            dic[nums[i]] = i
```

### 2. Max Distance Between Same Elements

**Problem:** Find maximum distance between two occurrences of the same element.

**Example:**

- Input: n = 12, arr = [3, 2, 1, 2, 1, 4, 5, 8, 6, 7, 4, 2]
- Output: 10
- Explanation: Maximum distance for element 2 is 11-1 = 10

**Code:**

```python
def maxDistance(self, arr, n):
    dic = {}
    ans = 0
    for i in range(n):
        if arr[i] in dic:
            ans = max(ans, abs(dic[arr[i]] - i))
        else:
            dic[arr[i]] = i
    return ans
```

### 3. Count Pairs with Given Sum

**Problem:** Count pairs of elements that sum to K.

**Example:**

- Input: N = 4, K = 6, arr[] = [1, 5, 7, 1]
- Output: 2

**Code:**

```python
def getPairsCount(arr, n, k):
    dic = {}
    ans = 0
    for i in arr:
        if k - i in dic:
            ans += dic[k - i]
        if i in dic:
            dic[i] += 1
        else:
            dic[i] = 1
    return ans
```

### 4. First Unique Character in String

**Problem:** Find the index of first non-repeating character.

**Example:**

- Input: s = "leetcode"
- Output: 0

**Code:**

```python
def firstUniqChar(self, s: str) -> int:
    dic = {}
    for i in s:
        if i in dic:
            dic[i] += 1
        else:
            dic[i] = 1

    for i in range(len(s)):
        if dic[s[i]] == 1:
            return i
    return -1
```

### 5. Longest Consecutive Sequence

**Problem:** Find the length of longest consecutive elements sequence.

**Example:**

- Input: nums = [100,4,200,1,3,2]
- Output: 4
- Explanation: Longest consecutive sequence is [1, 2, 3, 4]

**Code:**

```python
def longestConsecutive(self, nums: List[int]) -> int:
    present = {}
    checked = {}
    ans = 0

    for i in nums:
        present[i] = True

    for i in nums:
        if i not in checked and i-1 not in present:
            checked[i] = True
            temp = 1
            while i+1 in present:
                temp += 1
                checked[i+1] = True
                i = i + 1
            ans = max(ans, temp)

    return ans
```

### 6. Largest Subarray with 0 Sum

**Problem:** Find the length of largest subarray with sum equal to 0.

**Example:**

- Input: N = 8, A[] = [15,-2,2,-8,1,7,10,23]
- Output: 5
- Explanation: Subarray [-2, 2, -8, 1, 7] has sum 0

**Code:**

```python
def maxLen(self, n, arr):
    dic = {}
    sum_arr = 0
    ans = 0

    for i in range(n):
        sum_arr += arr[i]
        if sum_arr == 0:
            ans = i + 1
        elif sum_arr in dic:
            ans = max(ans, i - dic[sum_arr])
        else:
            dic[sum_arr] = i

    return ans
```

**Explanation:**

- If cumulative sum becomes 0, entire subarray from start has sum 0
- If same cumulative sum appears twice, the subarray between them has sum 0
- Store first occurrence of each sum to maximize subarray length

### 7. Largest Subarray of 0's and 1's

**Problem:** Find largest subarray with equal number of 0's and 1's.

**Example:**

- Input: N = 4, A[] = [0,1,0,1]
- Output: 4

**Code:**

```python
def maxLen(self, arr, N):
    dic = {}
    s = 0
    ans = 0

    for i in range(N):
        temp = -1 if arr[i] == 0 else 1
        s += temp

        if s == 0:
            ans = i + 1
        if s in dic:
            ans = max(ans, i - dic[s])
        else:
            dic[s] = i

    return ans
```

**Explanation:** Replace 0 with -1, then it becomes same as "largest subarray with 0 sum" problem.

### 8. Count Pairs with Absolute Difference K

**Problem:** Count pairs with absolute difference equal to K.

**Example:**

- Input: nums = [1,2,2,1], k = 1
- Output: 4

**Code:**

```python
def countKDifference(self, nums: List[int], k: int) -> int:
    dic = {}
    ans = 0

    for i in nums:
        if (i + k) in dic:
            ans += dic[(i + k)]
        if (i - k) in dic:
            ans += dic[i - k]

        if i in dic:
            dic[i] += 1
        else:
            dic[i] = 1

    return ans
```

**Explanation:**

- For absolute difference: |a - b| = k
- This gives us: a = b + k OR a = b - k
- Check for both conditions: (i + k) and (i - k)

### 9. Subarrays with Sum K

**Problem:** Count number of subarrays with sum equal to K.

**Example:**

- Input: N = 5, Arr = [10, 2, -2, -20, 10], k = -10
- Output: 3

**Code:**

```python
def findSubArraySum(self, arr, N, k):
    dic = {}
    s = 0
    ans = 0

    for i in range(N):
        s += arr[i]
        if s == k:
            ans += 1
        if s - k in dic:
            ans += dic[s - k]

        if s in dic:
            dic[s] += 1
        else:
            dic[s] = 1

    return ans
```

**Explanation:**

- If cumulative_sum - k exists in dictionary, there's a subarray with sum k
- Count all such occurrences

### 10. Longest Sub-Array with Sum K

**Problem:** Find length of longest subarray with sum K.

**Example:**

- Input: A[] = [10, 5, 2, 7, 1, 9], K = 15
- Output: 4
- Explanation: Subarray [5, 2, 7, 1] has sum 15

**Code:**

```python
def lenOfLongSubarr(self, arr, n, k):
    dic = {}
    s = 0
    ans = 0

    for i in range(n):
        s += arr[i]
        if s == k:
            ans = i + 1
        if s - k in dic:
            ans = max(ans, i - dic[s - k])
        if s not in dic:
            dic[s] = i

    return ans
```

**Explanation:**

- Only store first occurrence of each sum to maximize length
- For negative numbers, we don't update existing entries in dictionary

### 11. Longest Palindrome by Concatenating Two Letter Words

**Problem:** Find longest palindrome by concatenating two-letter words.

**Example:**

- Input: words = ["lc","cl","gg"]
- Output: 6
- Explanation: "lc" + "gg" + "cl" = "lcggcl"

**Code:**

```python
def longestPalindrome(self, words: List[str]) -> int:
    dic = {}
    flag = 0
    ans = 0

    for i in words:
        if i in dic:
            dic[i] += 1
        else:
            dic[i] = 1

    for word in words:
        if word[0] != word[1]:  # Different characters
            s = word[1] + word[0]  # Reverse
            if s in dic:
                ans += min(dic[word], dic[s]) * 4
            dic[word] = 0
            dic[s] = 0
        elif word[0] == word[1]:  # Same characters
            if dic[word] % 2 == 0:
                ans += dic[word] * 2
            else:
                if flag == 0:
                    ans += dic[word] * 2
                    flag = 1
                else:
                    ans += (dic[word] - 1) * 2
            dic[word] = 0

    return ans
```

### 12. Continuous Subarray Sum

**Problem:** Check if array has continuous subarray of size ≥ 2 whose sum is multiple of k.

**Example:**

- Input: nums = [23,2,4,6,7], k = 6
- Output: true
- Explanation: [2, 4] sums to 6, which is multiple of 6

**Code:**

```python
def checkSubarraySum(self, nums: List[int], k: int) -> bool:
    dic = {0: -1}  # Handle edge case
    s = 0

    for i in range(len(nums)):
        s += nums[i]
        remainder = s % k

        if remainder in dic:
            if i - dic[remainder] > 1:  # Size >= 2
                return True
        else:
            dic[remainder] = i

    return False
```

### 13. Subarray Sums Divisible by K

**Problem:** Count subarrays whose sum is divisible by K.

**Example:**

- Input: nums = [4,5,0,-2,-3,1], k = 5
- Output: 7

**Code:**

```python
def subarraysDivByK(self, nums: List[int], k: int) -> int:
    dic = {0: 1}  # Initialize with remainder 0
    s = 0
    count = 0

    for i in nums:
        s += i
        remainder = s % k
        if remainder in dic:
            count += dic[remainder]
            dic[remainder] += 1
        else:
            dic[remainder] = 1

    return count
```

**Explanation:**

- If two cumulative sums have same remainder when divided by k, the subarray between them is divisible by k
- Count all such occurrences

### 14. Additional Problems

#### Array Subset Check

```python
def isSubset(a1, a2, n, m):
    dic = {}
    for i in a1:
        if i in dic:
            dic[i] += 1
        else:
            dic[i] = 1

    for i in a2:
        if i in dic and dic[i] > 0:
            dic[i] -= 1
        else:
            return "No"
    return "Yes"
```

#### Hit Most Balloons (Points on Same Line)

```python
def mostBalloons(self, N, arr):
    if N < 3:
        return N

    ans = 0
    for i in range(N):
        x1, y1 = arr[i][0], arr[i][1]
        dic = {"inf": 0}
        count = 0

        for j in range(N):
            x2, y2 = arr[j][0], arr[j][1]
            if x1 == x2 and y1 == y2:
                count += 1
            else:
                if x2 - x1 == 0:
                    continue
                else:
                    slope = (y2 - y1) / (x2 - x1)
                    if slope in dic:
                        dic[slope] += 1
                    else:
                        dic[slope] = 1

        ans = max(ans, max(dic.values()) + count)

    return ans
```

## Key Patterns in Hashing Problems

### 1. **Two Sum Pattern**

- Use complement: `target - current_element`
- Store element with its index/count

### 2. **Subarray Sum Problems**

- Use cumulative sum
- Store sum with first occurrence index
- Check for `sum - target` in dictionary

### 3. **Counting Problems**

- Maintain frequency count
- Use dictionary to store counts

### 4. **Distance/Length Problems**

- Store first occurrence index
- Calculate distance using current index

### 5. **Palindrome Problems**

- Check for complements/reverses
- Handle odd/even frequencies

## Time and Space Complexity

- **Average Case:** O(1) for insertion, deletion, lookup
- **Worst Case:** O(n) when all elements hash to same location
- **Space Complexity:** O(n) for storing elements

## Best Practices

1. **Choose appropriate hash function** to minimize collisions
2. **Handle edge cases** like empty arrays, single elements
3. **Consider integer overflow** for large numbers
4. **Use appropriate data structures** (sets for existence, maps for counting)
5. **Initialize dictionaries** with base cases when needed
