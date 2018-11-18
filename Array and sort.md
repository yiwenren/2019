# Selection sort
In each round find the min of remaining numbers and put it in left side of array.
[-1, -10, 5, 2 3]
* iteration 1 : [-10 | -1, 5, 2, 3]
* iteration 2 : [-10, -1 | 5, 2, 3]
* iteration 3 : [-10, -1, 2 | 5, 3]
* iteration 4 : [-10, -1, 2, 3 | 5]

time complexity: O(n^2) => 1 + 2 + 3 + ... + n(n - 1)/2 => n^2 
```java
for (int i = 0; i < n - 1; i++) {
   for (int j = i + 1; j < n; j++)
}
```

https://www.lintcode.com/problem/sort-integers/description
```java
public void sortIntegers(int[] A) {
     // selection sort
     int length = A.length;
     for (int i = 0; i < length - 1; i++) {
         int min = i;
         for (int j = i + 1; j < length; j++) {
             if (A[j] < A[min]) {
                 min = j;
             }
         }
         int temp = A[min];
         A[min] = A[i];
         A[i] = temp;
     }
 }
```

Selection sort by two stacks:
```java
public void sortIntegers(int[] A) {
    // write your code here
    // selection sort with two Stacks
    
    if (A == null || A.length == 0) {
        return;
    }
    
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    for (int num : A) {
        stack1.push(num);
    }
    
    while(!stack1.isEmpty()) {
        int tempPeek = stack1.pop();
        // if tempPeek < stack2, push these number back to stack1
        while (!stack2.isEmpty() && tempPeek < stack2.peek()) {
            stack1.push(stack2.pop());
        }
        
        stack2.push(tempPeek);
        
    }
    
   for (int i = A.length - 1; i >= 0; i--) {
       A[i] = stack2.pop();
   }
}
```

# Merge sort
time complexity: O(nlog(n)) -> it will have log(n) levels, and in each level it will do comparison(O(1)) operation in n times. The total time complexity is n*log(n)

先局部有序再整体有序

https://www.lintcode.com/problem/sort-integers-ii/description
```java
public class Solution {
    /**
     * @param A: an integer array
     * @return: nothing
     */
    public void sortIntegers2(int[] A) {
        // write your code here
        // merge sort
        if (A == null || A.length == 0) {
            return;
        }
        
        int[] temp = new int[A.length];
        mergeSort(A, temp, 0, A.length - 1);
        
    }
    
    private void mergeSort(int[] A, int[] temp, int start, int end) {
        if (start >= end) {
            return;
        }
        int mid = start + (end - start)/2;
        mergeSort(A, temp, start, mid);
        mergeSort(A, temp, mid + 1, end);
        merge(A, temp, start, mid, end);
    }
    
    private void merge(int[] A, int[] temp, int start, int mid, int end) {
        int left = start, right = mid + 1;
        int index = start;
        // merge left and right part into temp
        while (left <= mid && right <= end) {
            if (A[left] < A[right]) {
                temp[index++] = A[left++];
            } else {
                temp[index++] = A[right++];
            }
        }
        
        // when the last loop ends, either left > mid or right > end
        // we need to merge the rest of left or right number into temp
        // left section is [start, mid], right section is [mid + 1, end]
        // so here it is left <= mid
        while (left <= mid) {
            temp[index++] = A[left++];
        }
        
        while (right <= end) {
            temp[index++] = A[right++];
        }
        
        // move int from temp to A
        index = start;
        while (index <= end) {
            A[index] = temp[index];
            index++;
        }
        
    }
}
```
