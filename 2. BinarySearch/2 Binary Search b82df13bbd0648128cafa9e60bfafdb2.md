# 2. Binary Search

- Mostly used in questions where they say array is sorted but sometimes it can be used in unsorted array but this is a very rare condition
- Every iteration choices/number of elements gets reduced to half
- TC → `O(log n)`

---

### Q > Finding index of first occurance of some element having true in an array

![Untitled](2%20Binary%20Search%20b82df13bbd0648128cafa9e60bfafdb2/Untitled.png)

- We will make 3 variables `si(starting idx) = 0`, `ei(ending idx) = array.length-1`, `firstOccurance = -1`
- while `si ≤ ei` we will run loop and calculate mid and perform operations as follows
- pseudo dry run with iteration number
    1. si = 0, ei = 16, mid = 8, fo = -1
    arr[mid] is false we will do `si = mid + 1`
    2.  si = 9, ei = 16, mid = 12, fo = 12
    possible answer found so we've updated the fo but we will still run the code to find
    if smaller value exist and do `ei = mid -1`
    3. si = 9, ei = 11, mid = 10, fo = 10
    arr[mid] is also true so updated fo, as it is a better possible answer and did `ei = mid - 1` to find better possible answer if exist
    4. si = 9, ei = 9, mid = 9, fo = 10
    value found at arr[mid] is false so we should make `si = mid + 1` but the loop will be broken at this point due to condition in while loop making the range invalid i.e best possible answer found is at idx 10
    5. we will return `fo`.
    

---

### Q> Search Element in a sorted array. (classical problem for this topic)

![Untitled](2%20Binary%20Search%20b82df13bbd0648128cafa9e60bfafdb2/Untitled%201.png)

- Algorithm:

```java
while(si <= ei) {
	calculate mid
	if(arr[mid] == target) return mid;
	else if(arr[mid] < target) si = mid + 1;
	else ei = mid - 1;
}
return -1; // not found
```

- SC: O(1) TC: O(log n)

---

[https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

[First and last occurrences of x | Practice | GeeksforGeeks](https://practice.geeksforgeeks.org/problems/first-and-last-occurrences-of-x3116/1)

- The intuition is simple we will create a variable of `possibleAns=-1` and create 2 seperate function for first and last position both will have binary search implemented in it but the only difference is the condition when we had the target, see both
- First Occurance
    - when the `target == nums[mid]` we will update the possibleAns and still continue the search in left side for smaller values so we will also do `ei = mid - 1`
    
    ```java
    public int firstPosition(int[] nums, int target) {
            int si = 0, ei = nums.length-1, possibleAns = -1;
            while(si <= ei) {
                int mid = si + (ei-si)/2;
    
                if(nums[mid] == target) {
                    possibleAns = mid;
                    ei = mid - 1;
                } else if(target > nums[mid]) {
                    si = mid + 1;
                } else {
                    ei = mid - 1;
                }
            }
            return possibleAns;
        }
    ```
    
- Last Occurance
    - when the `target == nums[mid]` we will update the possibleAns and still continue the search in right side for bigger values so we will also do `si = mid + 1`
    
    ```java
    public int lastPosition(int[] nums, int target) {
            int si = 0, ei = nums.length-1, possibleAns = -1;
            while(si <= ei) {
                int mid = si + (ei-si)/2;
    
                if(nums[mid] == target) {
                    possibleAns = mid;
                    si = mid + 1;
                } else if(target > nums[mid]) {
                    si = mid + 1;
                } else {
                    ei = mid - 1;
                }
            }
            return possibleAns;
        }
    ```
    

---

### Q> Given a sorted array of size “N”; find the index of the number in the array which is just greater than x and as close as possible to x.
Upper Bound (x) = Returns index of the number which is just greater than x and as close as possible to x.

- Input output 1
input → `nums = [3,5,5,8,8,10,12]` `x = 6`, i.e we have to find smallest upperbound to 6
output → 3 (`nums[3`]) we have to return the idx
- Input output 2
input → `nums = [1,1,3,3,5,500]` `x = 6`, i.e we have to find smallest upperbound to 6
output → 5 (`nums[5]`) we have to return the idx
- Algorithm:
    1. `nums[mid] ≤ target` -> search in right so `si = mid + 1`
    2. `nums[mid] > target` -> we want as closer as possible so we will check `num[mid-1] < nums[mid]` then we can easily go back and we will write `ei = mid`. If that is not the case then you’ve found the answer
    
    ```java
    public static int upperBound(int[] arr, int target) {
            int left = 0;
            int right = arr.length - 1;
     
            while (left <= right) {
                int mid = (left + right)/2 ; 
                if (arr[mid] <= target) {
                    left = mid + 1;
                } else {
                   if(mid==0){
                       return 0;
                   } else {
                   	   if(arr[mid-1]<=target){
                   	   	return mid; // mid is the lowest possible answer
                   	   } else {
                   	   	right = mid - 1; // possible ans exist so continue with smaller range
                   	   }
                   }
                }
            }
            return left;
        }
    ```
    

---

[https://leetcode.com/problems/peak-index-in-a-mountain-array/description/](https://leetcode.com/problems/peak-index-in-a-mountain-array/description/)

- Also known as BITONIC array cause it’s first increasing then decreasing
- Algorithm →
    1. we just need to apply binary search and compare the middle element to middle+1 element
    2. If the mid+1 is greater it means you’re in the increasing part of array so this may be the ans but we should search for better answers also so we will update our possiblePeak variable to mid+1 and make `si = mid+1` and continue searching in right part for bigger element
    3. Else we will do `ei = mid-1` because we ‘re in the decreasing part of the array so update the possiblePeak to mid ans search in left part for better answers

```java
public int peakIndexInMountainArray(int[] arr) {
        int possiblePeak = -1, n = arr.length;
        
        int si = 0, ei = n-1;
        while(si <= ei){
            int mid = si + (ei-si)/2;

            if(arr[mid] < arr[mid+1]) {
                possiblePeak = mid+1;
                si = mid+1;
            } else {
                possiblePeak = mid;
                ei = mid - 1;
            }
        }
        return possiblePeak;
    }
```

---

[https://leetcode.com/problems/find-in-mountain-array/description/](https://leetcode.com/problems/find-in-mountain-array/description/)

- Mixture of 2 Problems, we just need to find the peak element after that we will just apply binary search in 2 half compare the values, return the minimum idx.
- We just need to convert our binary search to order agnostic binary search so that it can work in both increasing and decreasing order(we’ll just keep a boolean for it and change comparision sign accordingly).

```java
// converted Binary search
public int binarySearch(MountainArray mountainArr, int target, int si, int ei, boolean increasing) {

        while(si <= ei) {
            int mid = si + (ei - si)/2;
            int midVal = mountainArr.get(mid);
            if(midVal == target) {
                return mid;
            } else {
                if(increasing) {
                    if(midVal < target) {
                        si = mid + 1;
                    } else {
                        ei = mid - 1;
                    }
                } else {
                    if(midVal > target) {
                        si = mid + 1;
                    } else {
                        ei = mid - 1;
                    }
                }
            }
        } 
        return -1;
    }

// main code
public int findInMountainArray(int target, MountainArray mountainArr) {
        int n = mountainArr.length();
        int peak = peakIndexInMountainArray(mountainArr, n);
        if(peak == -1) return mountainArr.get(0);
        int incIdx = binarySearch(mountainArr, target, 0, peak, true);
        int decIdx = binarySearch(mountainArr, target, peak + 1, n-1, false);
        if(incIdx == -1) return decIdx;
        else if(decIdx == -1) return incIdx;
        else return Math.min(incIdx, decIdx);
   }

// this also include peak founding function which is same as above question
```

---

[https://leetcode.com/problems/search-in-rotated-sorted-array/description/](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)

### Method 1(Not for duplicates) →

- We need a func to find the pivot then we will have 2 sorted arrays and we can do normal binary search in `0 to pivot` and `pivot+1 to n-1`

![Untitled](2%20Binary%20Search%20b82df13bbd0648128cafa9e60bfafdb2/Untitled%202.png)

- **case1** : whole arr is increasing in two parts then only time the subarray is decreasing is at `pivot` and `pivot+1` idx so will get the ans only when `arr[mid] > arr[mid+1]`. Then mid will become the pivot
- **case2**: There can also be a case where the middle element is smaller than the mid-1 element then the pivot will be mid - 1 element

![Untitled](2%20Binary%20Search%20b82df13bbd0648128cafa9e60bfafdb2/Untitled%203.png)

- **************case3:************** start element ≥ mid element, This basically means all the numbers after middle is smaller than start element i.e we can ignore all these element as we’re looking for max element so we do `ei = mid-1`

![Untitled](2%20Binary%20Search%20b82df13bbd0648128cafa9e60bfafdb2/Untitled%204.png)

- **************case4:************** start element < middle element, This means this is first sorted part of the array so we need to increase our start to `si = mid + 1` to find the max

```java
int findPivot(int[] arr){
        int start = 0, end = arr.length-1;
        
        while(start <= end){
            int mid = start + (end - start)/2;
            
            if(mid < end && arr[mid] > arr[mid+1]) return mid; // case 1
            if(mid > start && arr[mid] < arr[mid-1]) return mid-1; //case 2
            
            if(arr[mid] <= arr[start]) end = mid -1; //case 3
            else start = mid +1; //case 4
        }
        return -1;
}

// main code
public int search(int[] nums, int target) {
        int pivot = findPivot(nums);
        if(pivot == -1) return binarySearch(nums, target, 0, nums.length-1); // no pivot
// array is in increasing order from start to end we can apply simple binarySearch

        if(nums[pivot] == target) return pivot;
        if(target >= nums[0]) return binarySearch(nums, target, 0, pivot-1);
        return binarySearch(nums, target, pivot+1, nums.length-1);
 }

// this will also require the normal binary search func.
```

### Method 2 (with Duplicates) →

- we can have a problem when start end and mid all contains to same value

![Untitled](2%20Binary%20Search%20b82df13bbd0648128cafa9e60bfafdb2/Untitled%205.png)

- So our code will fail in finding the pivot in case 3 and 4 in above method
- In this what we can do is we can skip numbers from both the end by 1, i.e `start = start+1` and `end = end -1`

![Untitled](2%20Binary%20Search%20b82df13bbd0648128cafa9e60bfafdb2/Untitled%206.png)

we will ignore as many duplicates element as we could until we find the answer, so there will be a small change in the findPivot func but before ignoring we should also check once that are they contain the pivot element or not after that we can ignore them

```java
int findPivotWithDuplicates(int[] arr) {
        int start = 0;
        int end = arr.length - 1;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            
            if (mid < end && arr[mid] > arr[mid + 1]) {
                return mid;
            }
            if (mid > start && arr[mid] < arr[mid - 1]) {
                return mid-1;
            }

            // if elements at middle, start, end are equal then just skip the duplicates
            if (arr[mid] == arr[start] && arr[mid] == arr[end]) {
                // skip the duplicates
                // NOTE: what if these elements at start and end were the pivot??
                // check if start is pivot
                if (start < end && arr[start] > arr[start + 1]) {
                    return start;
                }
                start++;

                // check whether end is pivot
                if (end > start && arr[end] < arr[end - 1]) {
                    return end - 1;
                }
                end--;
            }
            // left side is sorted, so pivot should be in right
            else if(arr[start] < arr[mid] || (arr[start] == arr[mid] && arr[mid] > arr[end])) {
                start = mid + 1;
            } else {
                end = mid - 1;
            }
        }
        return -1;
    }
```

---

[Rotation | Practice | GeeksforGeeks](https://practice.geeksforgeeks.org/problems/rotation4723/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab)

- we just need to find the pivot and if pivot == -1 then return 0 otherwise return pivot + 1

```java
int findKRotation(int arr[], int n) {
        int pivot = findPivot(arr, n);
        if(pivot == -1) return 0;
        else return pivot+1;
    }
```

---