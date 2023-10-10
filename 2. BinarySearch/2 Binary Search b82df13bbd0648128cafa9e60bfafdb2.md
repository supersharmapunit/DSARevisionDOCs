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