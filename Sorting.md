# Sorting

- Arranging data in a particular permutation(this depends on requirement)

## Worst sorting approach

- eg - Given n integer values arrange them in increasing order. [15,-1,3,8,2,6]
    - brute force of the worst possible solution → we can generate all possible permutations i.e there will be (!n permutations) and filter out the required one. TC will be O(k*(!n)) where k is the TC to generate one permutation

## Selection Sort

- one of the most simplest algorithms
- Heap sort is extremly dependent on selection sort
- Given a situation
    1. In which array [” “,” “, “X”,” ”,Y,” “,” ”] green part is sorted in increasing order and red is arranged in some random order we need to sort.
        
        ![image](https://github.com/supersharmapunit/PlacementPrepDSA/assets/50194646/7dadad60-76ea-431d-9f11-9a0e1ef3f253)

        
    2. Lets say ‘X’ is the last element of the sorted area. And it is less than or equal(≤) to the minimum element of the region.
    3. Let’s say “Y” is the minimun element of the unsorted region. i.e X≤Y. So the Question we should try to solve till now is How can we expand the sorted region meanwhile decreasing the unsorted region.
    4. So we can conclude the fact that X is the largest element of the sorted area && the element  Y which is coming from the unsorted area is either bigger than X or equal to X (X≤Y this was the condition). Then it is self understood then is will also be bigger than the elements before X. eg →
        
        ![image](https://github.com/supersharmapunit/PlacementPrepDSA/assets/50194646/88f588e4-4b01-45fa-b846-e1874c955f25)

        
        So we can arrange or put Y just after X i.e we will swap 19 with 7 so our sorted will be expanded and unsorted region will shrink. And now X == 7. And we will repeat this process until X == arr.length-1.
        
    5. In the beginning of the algorithm the whole array will be the unsorted region and we will have to consider sorted region as -Infinity ( we will keep sorted region’s end pointer as -1)
    
        
        ![image](https://github.com/supersharmapunit/PlacementPrepDSA/assets/50194646/7badc4e7-3e42-4f4c-8e05-ba20e1c32c22)

        
        And after the first iteration we will get the sorted region as someting in the array. So the array will look like this after 1st iteration
        
        ![image](https://github.com/supersharmapunit/PlacementPrepDSA/assets/50194646/e046b8b4-1962-4012-9db8-ed1e2a3b0e0a)

        
    
    ### Algorithm:
    
    In every iteration: 
    
    1. We will get the index of the minimum element from the unsorted region
    
        
        ```java
        // This fn will return the idx of min. ele from arr in range [start, n-1]
        public static int getMinimumIndex(int[] arr, int start) {
        	int minIdx = start; // initially we assume that the stating idx is the min value
        	for(int i = start+1; i < arr.length; i++) {
        				// we go in the remaining array from [start+1, n-1]
        			if(arr[i] < arr[minIdx]) {
        					// compare if curr ele is better than the last found min ele
        					minIdx = i;
        			}
        	}
        	return minIdx;
        }
        ```
        
    2. Put the min ele obtained from the unsorted region into the last of sorted region.
        
        ```java
        public static void selectionSort(int[] arr) {
        	for(int i = 0; i < arr.length; i++) {
        		// i here denotes the unsorted region
        			int minIdx = getMinimumIndex(arr, i);
        			if(i != minIdx) swap(arr, i, minIdx);
        	}
        }
        ```
