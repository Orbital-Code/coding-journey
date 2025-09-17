# Heap Techniques - Complete Guide

## Identification

Jab bhi K define ho or minimum or maximum bhi present ho tab heap use hoga.

## Notes

- **Identification:** Jab max hoga tab min heap use hoga or jab min pucha hoga tab max heap use hoga
- **Time Complexity:** O(nlogn) se O(nlogk) tak optimize kre ga

## 1. Kth Smallest Element

**Solution:**

```python
from heapq import heapify, heappush, heappop

class Solution:
    def kthSmallest(self, arr, l, r, k):
        '''
        arr : given array
        l : starting index of the array i.e 0
        r : ending index of the array i.e size-1
        k : find kth smallest element and return using this function
        '''
        heap = []
        heapify(heap)
        for i in arr:
            heappush(heap, -1*i)
            if len(heap) > k:
                heappop(heap)

        return -heap[0]
```

**Theory:**
Jaise ki hummne malum hai ki heapq heap jo hi wo min heap hai toh hum isme negative kr k elements dale ge jisse ki kth largest element jo hai wo n-k th smallest element ho jai ga toh hum isko negative kr k save kre ge or 0th element return kr de ge.

## 2. Return K Largest Elements in Array

**Theory:**
Hum isme min heap banaye ge k size ka or fir sare elements dale ge or jab size k se jada hoga tab pop kr de ge or jab sare elements iterate ho jai ge tab hum, heap ko return kr de ge.

## 3. Nearly Sorted

**Solution:**

```python
from heapq import heapify, heappop, heappush

class Solution:
    #Function to return the sorted array.
    def nearlySorted(self, a, n, k):
        heap = []
        ans = []
        heapify(heap)
        for i in a:
            heappush(heap, i)

            if len(heap) > k:
                ans.append(heappop(heap))

        while len(heap) > 0:
            ans.append(heappop(heap))

        return ans
```

**Theory:**
Hum min heap me elements dalte rahe ge jab tak size k k equal na ho jai, once it is of size k hum new element ko push kr k pop operation ko call kr k popped element ko ans me append kr de ge. K sorted hai mtlb ki jo element ith pr hona chahiye wo use i+kth index me mile ga, i se i+k k beech me kahi bhi ho sakta hai is liye k size ka heap le ge jisse ki heap k size k liye sorted rahe ga or pop kr k humme sabse chota element milte jai ga. Or sare element pop kr k ans me dalne k bar ans return kr de ge.

## 4. K Closest Numbers

**Solution:**

```python
from heapq import heapify, heappop, heappush

class Solution(object):
    def findClosestElements(self, arr, k, x):
        """
        :type arr: List[int]
        :type k: int
        :type x: int
        :rtype: List[int]
        """
        heap = [[]]
        heapify(heap)
        for i in arr:
            temp = abs(i-x)
            heappush(heap, [-temp, -i])

            if len(heap) > k:
                heappop(heap)
        # print(heap)
        ans = [None]*k
        c = 0
        while len(heap) > 0:
            ans[c] = (-heappop(heap)[1])
            c += 1
        ans.sort()
        return ans
```

**Theory:**
Hum ek heap banaye ge lists ki, jisme hum difference or element dal sake. Pehle absolute difference nikale ge or usko negative kr k heap me dale ge kyu ki humko max heap ki requirement hai na ki min heap ki or uske sath element ko bhi negative kr k dale ge kyu ki ye first agr same hai toh second element k hisab se min heap lagaye ge or humko start ke k elements chahiye honge toh -ve kre ge take max heap lag jai or start k elements mile.

## 5. Top K Frequent Numbers

**Solution:**

```python
from heapq import heappop, heappush, heapify

class Solution(object):
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        dic = {}
        heap = [[]]
        heapify(heap)
        for i in nums:
            if i in dic:
                dic[i] += 1
            else:
                dic[i] = 1
        for i in dic:
            heappush(heap, [dic[i], i])
            if len(heap) > k:
                heappop(heap)

        ans = []
        while len(heap) > 0:
            ans.append(heappop(heap)[1])
        return ans
```

**Theory:**
Refer above one

## 6. Frequency Sort

**Solution:**

```python
from heapq import heappush, heappop, heapify

class Solution(object):
    def frequencySort(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        dic = {}
        for i in nums:
            if i in dic:
                dic[i] += 1
            else:
                dic[i] = 1

        heap = []
        heapify(heap)
        for i in dic:
            heappush(heap, [dic[i], -i])

        ans = []
        print(heap)
        while len(heap) > 0:
            temp = heappop(heap)
            # print(temp)
            arr = [-temp[1]]*temp[0]
            ans = ans + arr
        return ans
```

**Theory:**
Refer above
