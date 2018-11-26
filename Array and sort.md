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

同类型题：
A1B2C3D4 -> ABCD1234 和merge sort是一样的，只是字符先后的规则稍微变一下。

那么，如果从ABCD1234 -> A1B2C3D4? AB CD 12 34 ->(reverse 23) AB12 CD34 ->(reverse 23) AA1B2 C3D4

# Quick Sort
先整体有序再局部有序
Average time complexity: O(nlog(n))
Worst time complexity: O(n*n) - 比方说Pivot选在了最后一位，如果原本数组就是有序的，那么分区的时候左边或右边就有一边是没有数的，就等于要做(n-1) + (n-2) + ... + 1次对比操作，O(n*n). best case是分区的时候把左右两边分成相等的，就是一共logn层，每一层做n次操作，一共nlogn
```java
public int[] quickSort(int[] array) {
    // Write your solution here
    if (array == null || array.length == 0) {
      return array; 
    }
    
    sort(array, 0, array.length - 1);
    return array;
  }
  
  private void sort(int[] array, int start, int end) {
    if (start >= end) {
      return;
    }
    
    int left = start, right = end;
    int mid = (left + right) / 2;
    
    // key point 2: every time you compare left & right, it should be 
    // left <= right not left < right
    while(left <= right) {
      // key point 1: pivot is the value, not the index
      while(left <= right && array[left] < array[mid]) {
        left++;
      }
      while(left <= right && array[right] > array[mid]) {
        right--; 
      }
      if (left <= right) {
        int temp = array[left];
        array[left] = array[right];
        array[right] = temp;
        left++;
        right--;
      }
    }
    
    sort(array, start, right);
    sort(array, left, end); 
  }

```

# Rainbow sort
array[0,i) 都是-1, array(i, j] 是0, array[j, k] 是未知, array(k, end]是1
j往前移动，碰到-1就与i位置的数字交换，然后j与i都继续往前移动一位，保证[0, i)是-1; 如果碰到0则j继续往前移动，保证[i,j)都是0;如果碰到1则交换j与k位置上的数，之后j向左移动一位，使得1都被抛在了j的右边, 但是j不移动，因为换过来的数依旧是未知，j一直到下次找到了-1， 0再移动，使得[j, k]是未知。

```java
public int[] rainbowSort(int[] array) {
    // Write your solution here.
    //keep -1 in array[0,i), 0 in array(i, j], unknown in array[j, k], 1 in array(k, end]
    int i = 0, j = 0, k = array.length - 1;
    while (j <= k) {
      if (array[j] == -1) {
        swap(array, i++, j++);
      } else if (array[j] == 0) {
        j++; 
      } else {
        swap(array, j, k--); 
      }
    }
    
    return array;
  }
  
  private void swap(int[] array, int left, int right) {
    int temp = array[left];
    array[left] = array[right];
    array[right] = temp;
  }
```
