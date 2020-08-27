# Search-Sort

## [MIT6.0001 lec12](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/lecture-videos/lecture-12-searching-and-sorting/)
### Search algorithms
**1. Linear search: complexity - O(n)**

(1) On unsorted list
```
def linear_search(L, e):
    found = False
    for i in range(len(L)):
        if e == L[i]:
            found = True
    return found
```

(2) On sorted list
```
def search(L, e):
    for i in range(len(L)):
        if L[i] == e:
            return True
        if L[i] > e:
            return False
    return False
```

**2. Bisection search (assume list is sorted, complexity - O(log n)**
```
def bisect_search(L, e):
    def bisect_search_helper(L, e, low, high):
        if high == low:
            return L[low] == e
        mid = (low + high)//2
        if L[mid] == e:
            return True
        elif L[mid] > e: #search the left side
            if low == mid: #nothing left to search
                return False
            else:
                return bisect_search_helper(L, e, low, mid - 1)
        else:  #search the right side
            return bisect_search_helper(L, e, mid + 1, high)
    if len(L) == 0:
        return False
    else:
        return bisect_search_helper(L, e, 0, len(L) - 1)
```

### Sort algorithms
**1. Monkey sort(bogosort)**

randomly shuffle until sorted
```
def bogo_sort(L):
    while not is_sorted(L):
        random.shuffle(L)
```
Complexity:
- best case: O(n)
- worst case:  O(?)

**2. Bubble sort**
- compare consecutive pairs of elements
- swap elements in pair such that smaller is first
- when reach end of list, start over again
- stop when no more swaps have been made
```
def bubble_sort(L):
    swap = False
    while not swap:  #for passing
        swap = True
        for j in range(1, len(L)): # for comparing
            if L[j-1] > L[j]: # swap if the former is smaller
                swap = False
                temp = L[j]
                L[j] = L[j-1]
                L[j-1] = temp
```
Complexity: O(n^2)

**3. Selection sort**

always extract the minimum element in the remaining sublist and swap it with the first element in this sublist
```
def selection_sort(L):
    suffixSt = 0
    while suffixSt != len(L):
        for i in range(suffixSt, len(L)):
            if L[i] < L[suffixSt]:
                L[suffixSt], L[i] = L[i], L[suffixSt]
        suffixSt += 1
```
Complexity: O(n^2)

**4. Merge Sort**

use a divide-and-conquer approach
- split list in half until sublist of only 1 element
- merge such that sublists will be sorted after merge
```
#merging sublists step
def merge(left, right):  
    result = []
    i,j = 0,0
    while i < len(left) and j < len(right):
        if left[i] < right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    while (i < len(left)): # when right sublist id empty
        result.append(left[i])
        i += 1
    while (j < len(right)): # when left sublist id empty
        result.append(right[j])
        j += 1
    print('merge: ' + str(left) + '&' + str(right) + ' to ' +str(result))
    return result
    
#merge sort algorithm
def merge_sort(L): 
    print('merge sort: ' + str(L))
    if len(L) < 2:  # base case
        return L[:]
    else:
        middle = len(L)//2
        left = merge_sort(L[:middle])
        right = merge_sort(L[middle:])
        return merge(left, right)
```
complexity O(n log n)
