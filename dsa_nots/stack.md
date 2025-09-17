# Stack Techniques - Complete Guide

## Pattern

If the brute force approach has complexity of n^2 and second loop has dependency on i means second loop either starts from i or ends on i, we will use stack to reduce time complexity.

## Questions

### 1. Next Greater Element I

**Input:** nums1 = [4,1,2], nums2 = [1,3,4,2]  
**Output:** [-1,3,-1]  
**Explanation:** The next greater element for each value of nums1 is as follows:

- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

**Solution:**

```python
def nextGreaterElement(self, nums1, nums2):
    """
    :type nums1: List[int]
    :type nums2: List[int]
    :rtype: List[int]
    """
    st = []
    temp = []
    ans = []
    for i in range(len(nums2)-1, -1, -1):
        if not st:
            st.append(nums2[i])
            temp.append(-1)
        else:
            while st:
                if st[-1] > nums2[i]:
                    temp.append(st[-1])
                    st.append(nums2[i])
                    break
                else:
                    st.pop()
            if len(st) == 0:
                temp.append(-1)
                st.append(nums2[i])

    for i in nums1:
        ans.append(temp[-(nums2.index(i)+1)])
    return ans
```

**Points:**
Main array ko last se traverse kr k check kre ge ki usse bda element kon sa hai stack me, pop krte jai ge or jaha mile ga usko temp me save kra le ge or agr nahi mila toh -1 save kraye ge or stack me us element ko daal de ge kyuki wo ab tak ka sabse bda element hoga.
Fir jin jin element k liye pucha hai uske index me +1 kr k last se value return kr de ge, temp me values peeche se hongi kyu ki main array ko ulta traverse kiya hai.

Isi tahrah se hum Nearest Smaller to Right, Nearest Smaller to Left and Nearest Largest to Left inko bhi kre ge.

### 2. Online Stock Span

**Input:**  
["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]  
[[], [100], [80], [60], [70], [60], [75], [85]]  
**Output:**  
[null, 1, 1, 1, 2, 1, 4, 6]

**Explanation:**  
StockSpanner stockSpanner = new StockSpanner();  
stockSpanner.next(100); // return 1  
stockSpanner.next(80); // return 1  
stockSpanner.next(60); // return 1  
stockSpanner.next(70); // return 2  
stockSpanner.next(60); // return 1  
stockSpanner.next(75); // return 4, because the last 4 prices (including today's price of 75) were less than or equal to today's price.  
stockSpanner.next(85); // return 6

**Solution:**

```python
class StockSpanner(object):
    def __init__(self):
        self.st = []
        self.temp = []
        self.big = 0

    def next(self, price):
        """
        :type price: int
        :rtype: int
        """
        if len(self.st) == 0:
            self.st.append(price)
            self.temp.append(1)
            self.big = price
            return 1
        else:
            if self.st[-1] > price:
                self.st.append(price)
                self.temp.append(1)
                return 1
            elif price > self.big:
                self.st.append(price)
                self.temp.append(len(self.st))
                self.big = price
                return len(self.st)
            elif self.st[-1] == price:
                self.st.append(price)
                self.temp.append(self.temp[-1]+1)
                return self.temp[-1]
            else:
                temp = 1
                while temp <= len(self.st) and self.st[-temp] <= price:
                    temp += 1
                self.st.append(price)
                self.temp.append(temp)
                return temp
```

Is me apan 1 stack maintain kr rahe hai usme sare element append kre ge or fir last element se check kre ge, agr use chota hai toh 1 return kr de ge agar brabar hai toh uske uske stock span me 1 increment kr k return kr de ge. Ya fir sabse bda hai agr toh length ko hi return kr de. Or agar sabse koi beech ka hai toh backtrack kr k check kre ge har element se or fir desired value return kre ge.

**Better approach:**

```python
class StockSpanner(object):
    def __init__(self):
        self.st = []
        self.temp = 0

    def next(self, price):
        """
        :type price: int
        :rtype: int
        """
        self.temp += 1
        dic = {"ts": self.temp, "price": price}

        while self.st and self.st[-1]["price"] <= price:
            self.st.pop()

        self.st.append(dic)
        if len(self.st) > 1:
            return self.st[-1]["ts"] - self.st[-2]["ts"]
        return self.temp
```

Isme hum 1 stack or 1 index variable banaye ge stack me hum current price or uska index dale ge.
Hum current price ko last price se compare kre ge, agr wo last price se chota hai toh usko direct append kr de ge, or agr current price bada hai toh usse chote walo ka koi use nahi hi kyu ki loop usi pr aa k ruk jai ga, toh usse chote pop kre ge or usko append kre ge.
Last me, last me se second last ka index subtract kr k ans return kr de ge.

### 3. Largest Rectangle in Histogram

**Input:** heights = [2,1,5,6,2,3]  
**Output:** 10  
**Explanation:** The above is a histogram where width of each bar is 1. The largest rectangle is shown in the red area, which has an area = 10 units.

**Solution:**

```python
class Solution(object):
    def nsl(self, arr):
        st = []
        temp = [i for i in arr]
        for i in range(len(arr)):
            dic = {"ind": i, "value": arr[i]}
            if not st:
                temp[i] = -1
                st.append(dic)
                continue
            if st[-1]["value"] < arr[i]:
                temp[i] = st[-1]["ind"]
                st.append(dic)
            else:
                while st:
                    if st and st[-1]["value"] >= arr[i]:
                        st.pop()
                    else:
                        break
                if st:
                    temp[i] = st[-1]["ind"]
                    st.append(dic)
                else:
                    temp[i] = -1
                    st.append(dic)
        return temp

    def nsr(self, arr):
        st = []
        temp = [i for i in arr]

        for i in range(len(arr)-1, -1, -1):
            dic = {"ind": i, "value": arr[i]}
            if not st:
                temp[i] = len(arr)
                st.append(dic)
            elif st[-1] < arr[i]:
                temp[i] = st[-1]["ind"]
                st.append(dic)
            else:
                while st:
                    if st[-1]["value"] >= arr[i]:
                        st.pop()
                    else:
                        break
                if st:
                    temp[i] = st[-1]["ind"]
                else:
                    temp[i] = len(arr)
                st.append(dic)
        return temp

    def largestRectangleArea(self, height):
        """
        :type heights: List[int]
        :rtype: int
        """
        w = [i for i in heights]
        ans = 0
        t1 = self.nsl(heights)
        t2 = self.nsr(heights)
        for i in range(len(t1)):
            w[i] = abs(t1[i] - t2[i]) - 1
        print(w)
        for i in range(len(heights)):
            ans = max(ans, w[i] * heights[i])
        return ans
```

**Explanation:**
Humko current index pr k reh k uske left or right me jo bhi histogram ki buildings hai usme me se smaller wale ko pick krna jaha per use chota mile ge mtlb waha tak us height ka rectangle form ho skta hai. Is tarah se hum ko sare pillar ki width mil jai gi, fir hum uski width ko uski height se multiply kr k area nikal le ge, or max area return kr de ge.

### 4. Maximal Rectangle

**Input:** matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]  
**Output:** 6  
**Explanation:** The maximal rectangle is shown in the above picture

**Solution:**

```python
class Solution(object):
    def mah(self, arr):
        st = []
        nsr = [0 for i in arr]
        nsl = [0 for i in arr]
        n = len(arr)
        st = []
        # nsr
        for i in range(n-1, -1, -1):
            while st:
                if arr[st[-1]] >= arr[i]:
                    st.pop()
                else:
                    break

            if not st:
                nsr[i] = n
            else:
                nsr[i] = st[-1]

            st.append(i)
        st = []
        for i in range(n):
            while st:
                if arr[st[-1]] >= arr[i]:
                    st.pop()
                else:
                    break
            if not st:
                nsl[i] = -1
            else:
                nsl[i] = st[-1]

            st.append(i)

        ans = 0
        for i in range(n):
            temp = arr[i] * (nsr[i] - nsl[i] - 1)
            ans = max(ans, temp)
        return ans

    def maximalRectangle(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        ans = 0
        var = [0 for i in matrix[0]]
        for i in matrix:
            for j in range(len(i)):
                if int(i[j]) != 0:
                    var[j] = var[j] + int(i[j])
                else:
                    var[j] = 0
            temp = self.mah(var)

            ans = max(ans, temp)
        return ans
```

**Solution:**
Humne is question ko maximum area of histogram(ques above) me convert kr liya hai. Hum 2D array ko 1D me change kre ge or fir max area of histogram lga k max area nikal le ge. Conversion k liye hum row wise chale ge, first row ko as it is pick kre ge, or second row se agr first row k ith index pr koi value hai other than 0 than we will add our current ith index to prev one else we will keep 0 as it is(for more clarity watch AV).

### 5. Trapping Rain Water

**Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]  
**Output:** 6  
**Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Solution:**

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        ans = 0
        l = len(height)
        mxr = [0] * l
        mxl = [0] * l
        for i in xrange(l):
            mxl[i] = max(mxl[i-1], height[i])

        for i in xrange(l-1, -1, -1):
            mxr[i] = max(mxr[(i+1) % l], height[i])
        for i in xrange(l):
            dif = min(mxr[i], mxl[i]) - height[i]
            if dif > 0:
                ans += dif

        return ans
```

**Solution:**
Isme hum current element k left max element ko nikale ge or right max element ko nikale ge, fir dono me se minimum le ge or current ki height se subtract kr de ge toh mil jai ga ki kitna pani aa skta hai. For more clarity check AV.

### 6. Count the Reversals

**Solution:**

```python
def countRev(s):
    # your code here
    if len(s) % 2 != 0:
        return -1

    st = []
    for i in s:
        if not st:
            st.append(i)
        elif i == "{":
            st.append(i)
        else:
            if st[-1] == "{":
                st.pop()
            else:
                st.append(i)

    stack = []
    ans = 0
    for i in st:
        if not stack and i == "}":
            stack.append("{")
            ans += 1
        elif not stack:
            stack.append(i)
        else:
            if stack[-1] == "{" and i == "}":
                stack.pop()
            elif stack[-1] == "{" and i == "{":
                stack.pop()
                ans += 1
    return ans
```

**Theory:**
Hum pehle 1 stack(st) banaye ge or balanced wale hta k baki usme append kr de ge, us stack me append krne k liye hum last stack element ko check re ge,
Jab hame 1 stack mil jai ga unbalanced parentheses ka tab hum ek or stack banaye ge or pairs me sab ko balance krte jai ge, kyu ki pura unbalance hoga toh pair me krna bhi optimized hi rahe ga, kyu ki jo bhi possible pair ban skta tha wo remove ho chuka hoga.

### 7. Longest valid Parentheses

**Solution:**

```python
class Solution:
    def maxLength(self, s):
        indx = [-1]
        ans = 0
        for i in range(len(s)):
            if s[i] == "(":
                indx.append(i)
            else:
                indx.pop()
                if not indx:
                    indx.append(i)
                else:
                    ans = max(ans, i - indx[-1])
        return ans
```

**Theory:**
Isme hum sirf opening bracket k index save kre ge stack me or jab humko closing bracket mile ga tab hum last index ko pop kr de ge or uske baad agr st empty ho jata hai toh humko wo index save krna hoga kyu ki fir hum us index se distance measure kre ge.
Jab pop krene k bad st empty nahi hoga tab hum current index se last index ko subtract kr k length nikal le ge.

### 8. Stack Permutations

**Solution:**

```python
class Solution:
    def isStackPermutation(self, N: int, A: List[int], B: List[int]) -> int:
        # code here
        st = []
        j = 0
        for i in range(N):
            st.append(A[i])

            while st and st[-1] == B[j]:
                st.pop()
                j += 1

        if st == []:
            return 1
        return 0
```

**Theory:**
The idea to start iterating on the input array and storing its element one by one in a stack and if the top of our stack matches with an element in the output array we will pop that element from the stack and compare the next element of the output array with the top of our stack if again it matches then again pop until our stack isn't empty

### 9. Sort a stack

**Solution:**

```python
class Solution:
    def insert(self, s, var):
        if len(s) == 0 or s[-1] <= var:
            s.append(var)
            return
        temp = s[-1]
        s.pop()
        self.insert(s, var)
        s.append(temp)
        return

    def Sorted(self, s):
        # Code here
        if len(s) == 1:
            return
        temp = s.pop()
        self.Sorted(s)
        self.insert(s, temp)
```

**Theory:**
We will pop last element and sort the result array and then sort add last element in sorted array recursively.
