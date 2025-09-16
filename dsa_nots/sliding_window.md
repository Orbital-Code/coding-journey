# Sliding Window Techniques - Complete Guide

## Introduction

Sliding Window is a technique used to solve array/string problems efficiently by maintaining a window of elements and sliding it through the data structure. This guide covers identification, types, and practical examples.

## Fixed Size Sliding Window Problems

### 1. Maximum Sum Subarray of Size K

**Problem:** Find the maximum sum of any subarray of size K.

**Example:**

- Input: N = 4, K = 2, Arr = [100, 200, 300, 400]
- Output: 700
- Explanation: Arr[2] + Arr[3] = 300 + 400 = 700 (maximum possible)

**Code:**

```python
class Solution:
    def maximumSumSubarray(self, k, arr, N):
        i = 0
        j = 0
        ans = 0
        s = 0
        while j < N:
            s = s + arr[j]
            if (j - i + 1) < k:
                j += 1
            else:
                ans = max(ans, s)
                s = s - arr[i]
                i += 1
                j += 1
        return ans
```

**Explanation:**
We use two pointers `i` and `j` as start and end of the sliding window. We expand the window until it reaches size K, then we check for maximum sum and slide the window by removing `arr[i]` and moving both pointers forward.

### 2. First Negative Integer in Every Window of Size K

**Problem:** Find the first negative integer in every window of size K.

**Example:**

- Input: N = 5, A[] = {-8, 2, 3, -6, 10}, K = 2
- Output: -8 0 -6 -6
- Explanation:
  - {-8, 2} = -8
  - {2, 3} = 0 (no negative integer)
  - {3, -6} = -6
  - {-6, 10} = -6

**Code:**

```python
def printFirstNegativeInteger(A, N, K):
    i, j = 0, 0
    cache = []
    result = []
    while j < N:
        if A[j] < 0:
            cache.append(A[j])
        if (j - i + 1) < K:
            j += 1
        else:
            if len(cache) == 0:
                result.append(0)
            else:
                result.append(cache[0])
                if cache[0] == A[i]:
                    cache.pop(0)
            i += 1
            j += 1
    return result
```

**Explanation:**
We maintain a cache to store negative numbers in the current window. When the window size reaches K, we append the first negative number (or 0 if none exists) to the result. We remove elements from the cache when they go out of the window.

### 3. Count Occurrences of Anagrams

**Problem:** Count occurrences of anagrams of a pattern in a text.

**Example:**

- Input: txt = "forxxorfxdofr", pat = "for"
- Output: 3
- Explanation: "for", "orf", and "ofr" are anagrams of the pattern

**Code:**

```python
class Solution:
    def search(self, pat, txt):
        i = 0
        j = 0
        ans = 0
        n = len(txt)
        k = len(pat)
        dic = {}
        dic1 = {}

        # Create frequency map for pattern
        for let in pat:
            if let in dic1:
                dic1[let] += 1
            else:
                dic1[let] = 1

        while j < n:
            if txt[j] in dic:
                dic[txt[j]] += 1
            else:
                dic[txt[j]] = 1

            if j - i + 1 < k:
                j += 1
            else:
                flag = 1
                for let in dic1:
                    if let not in dic or dic1[let] != dic[let]:
                        flag = 0

                if flag == 1:
                    ans += 1

                dic[txt[i]] -= 1
                i += 1
                j += 1

        return ans
```

**Explanation:**
We first create a frequency map of the pattern. Then we use sliding window to maintain frequency of current window characters. When window size equals pattern length, we compare frequencies to check if it's an anagram.

### 4. Sliding Window Maximum

**Problem:** Find the maximum element in every window of size K.

**Example:**

- Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
- Output: [3,3,5,5,6,7]

**Code:**

```python
from collections import deque

class Solution:
    def maxSlidingWindow(self, arr: List[int], B: int) -> List[int]:
        i = 0
        j = 0
        ans = []
        st = deque()
        while j < len(arr):
            while st and st[-1] < arr[j]:
                st.pop()
            st.append(arr[j])
            if j - i + 1 < B:
                j += 1
            else:
                ans.append(st[0])
                if st[0] == arr[i]:
                    st.popleft()
                i += 1
                j += 1
        return ans
```

**Explanation:**
We use a deque to maintain elements in decreasing order. We remove smaller elements from the right when a larger element comes, as they can never be maximum. The front of deque always contains the maximum element of current window.

## Variable Size Sliding Window Problems

### 5. Longest K Unique Characters Substring

**Problem:** Find the longest substring with exactly K unique characters.

**Example:**

- Input: S = "aabacbebebe", K = 3
- Output: 7
- Explanation: "cbebebe" is the longest substring with 3 distinct characters

**Code:**

```python
class Solution:
    def longestKSubstr(self, s, k):
        i = 0
        j = 0
        dic = {}
        ans = -1
        n = len(s)

        while j < n:
            if s[j] in dic:
                dic[s[j]] += 1
            else:
                dic[s[j]] = 1

            if len(dic) == k:
                ans = max(ans, j - i + 1)
            else:
                while len(dic) > k:
                    dic[s[i]] -= 1
                    if dic[s[i]] == 0:
                        del dic[s[i]]
                    i += 1
            j += 1
        return ans
```

**Explanation:**
We expand the window by adding characters and track their frequency. When we have exactly K unique characters, we update the answer. When we exceed K characters, we shrink the window from the left until we have exactly K characters.

### 6. Longest Substring Without Repeating Characters

**Problem:** Find the length of the longest substring without repeating characters.

**Example:**

- Input: s = "abcabcbb"
- Output: 3
- Explanation: "abc" is the longest substring without repeating characters

**Code:**

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        i = 0
        j = 0
        dic = {}
        ans = 0
        n = len(s)
        while j < n:
            if s[j] not in dic:
                dic[s[j]] = True
                ans = max(ans, j - i + 1)
            else:
                while s[j] in dic:
                    del dic[s[i]]
                    i += 1
                dic[s[j]] = True
            j += 1
        return ans
```

**Explanation:**
We expand the window and track characters in a dictionary. When we encounter a duplicate character, we shrink the window from the left until the duplicate is removed, then continue expanding.

## Additional Problems to Explore

### 7. Count Number of Substrings Having At Least K Distinct Characters

This problem involves counting substrings that have at least K distinct characters.

### 8. Number of Subarrays Having Sum Less Than K

This problem involves counting subarrays whose sum is less than a given value K.

## General Approach for Sliding Window Problems

### Fixed Size Window:

1. Calculate window sum/condition for first window
2. Slide the window: remove first element, add next element
3. Update result for each window position

### Variable Size Window:

1. Expand window by moving right pointer
2. Check if current window satisfies condition
3. If condition violated, shrink window from left
4. Update result when condition is satisfied

## Key Points to Remember

- **Two Pointers:** Use `i` (start) and `j` (end) to define window boundaries
- **Condition Checking:** Always check if current window satisfies the problem condition
- **Window Sliding:** Move pointers appropriately to maintain or find optimal window
- **Data Structures:** Use appropriate data structures (arrays, hashmaps, deques) to track window state
- **Time Complexity:** Most sliding window problems can be solved in O(n) time
