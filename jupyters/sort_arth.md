

```python
"""
插入排序
插入核心思想就是将一个数据插入到已排序好的有序数组中
该有序数组从index=0开始，后面的n-1位每次都与有序数组的最后一位进行比较，依次往前
时间效率为n^2
"""
def insert_sort(lists):
    count = len(lists)
    for i in range(1, count):
        key = lists[i]
        j = i - 1
        while j >= 0:
            if lists[j] > key:
                lists[j + 1] = lists[j]
                lists[j] = key
            j -= 1
    return lists

input_list = [7,5,8,1,4]
output_list = []
output_list = insert_sort(input_list)
print(output_list)
```

    [1, 4, 5, 7, 8]



```python
"""
希尔排序
是插入排序的变种，对数列按下标的一定增量 t 进行分组(1,3,5)(2,4)，随着t->1，整个数列被分为一组
时间效率取决于t的选取，一般为o(n^(3/2))
注:python3 中 // 表示整除
"""
def shell_sort(lists):
    count = len(lists)
    group = 2
    t = count // group
    while t > 0:
        for i in range(0, t):
            j = i + t
            while j < count:
                if lists[j - t] > lists[j]:
                    lists[j - t],lists[j] = lists[j],lists[j - t]
                j += t
        t -= 1
    return lists

input_list = [7,5,8,1,4]
output_list = []
output_list = shell_sort(input_list)
print(output_list)
```

    [1, 4, 5, 7, 8]



```python
"""
冒泡排序
重复得走过要排序的数列，一次比较相邻的2个元素，如果顺序错误就进行交换
网上流传很多的版本是错误的，不是冒泡排序
"""
def bubble_sort(lists):
    count = len(lists)
    for i in range(count - 1, 0, -1):
        for j in range(i):
            if lists[j] > lists[j + 1]:
                lists[j],lists[j + 1] = lists[j + 1],lists[j]
    return lists

input_list = [7,5,8,1,4,3,55,91,30]
output_list = []
output_list = bubble_sort(input_list)
print(output_list)
```

    [1, 3, 4, 5, 7, 8, 30, 55, 91]



```python
"""
快速排序
（1）通过基准值k将原数列分为2部分，小于k的放在左边，大于k的放在右边，
（2）对左边和右边子序列分别进行（1）中操作直到子序列长度为1
关于基准值的选取，本例中默认选最左边
o(n log n)
"""
def quick_sort(lists, left, right):
    if left >= right:
        return lists
    start = left
    end = right
    count = len(lists)
    key = lists[left]
    while left < right:
        while left < right and lists[right] >= key:
            right -= 1
        if left < right:
            lists[left] = lists[right]
        while left < right and lists[left] <= key:
            left += 1
        if left < right:
            lists[right] = lists[left]
    lists[right] = key
    quick_sort(lists, start, right - 1)
    quick_sort(lists, right + 1, end)
    return lists

    
input_list = [7,5,8,1,4,3,55,91,30]
output_list = []
output_list = quick_sort(input_list, 0, len(input_list) - 1)
print(output_list)

```

    [1, 3, 4, 5, 7, 8, 30, 55, 91]



```python
"""
直接选择排序
进行n^2次交换，每次从待排序数列r[i]~r[n]中选出最小的记录与r[i]进行交换，然后i++
o(n^2)
"""
def select_sort(lists):
    count = len(lists)
    for i in range(0, count):
        min = i
        for j in range(i, count):
            if lists[min] > lists[j]:
                min = j
        lists[i],lists[min] = lists[min],lists[i]
    return lists

input_list = [7,5,8,1,4,3,55,91,30]
output_list = []
output_list = select_sort(input_list)
print(output_list)
```

    [1, 4, 5, 7, 8]



```python
"""
堆排序
利用最大值堆或者最小值堆进行排序，每次的堆顶数据即为最大值或者最小值
比如从小到大排序，建立最大根堆的实例如下
o(n log n)
"""

# 根据最大值堆的规则进行调整，既根节点大于左右子树
def heap_adjust(lists, i ,size):
    left_child = i * 2 + 1
    right_child = i * 2 + 2
    maxIndex = i
    if i < (size // 2):
        if left_child < size and lists[left_child] > lists[maxIndex]:
            maxIndex = left_child
        if right_child < size and lists[right_child] > lists[maxIndex]:
            maxIndex = right_child
        if maxIndex != i:
            lists[i],lists[maxIndex] = lists[maxIndex],lists[i]
            heap_adjust(lists, maxIndex, size)

# 建堆
def heap_build(lists, size):
    for i in range(0, int(size//2))[::-1]:
        heap_adjust(lists, i, size)
    
def heap_sort(lists):
    size = len(lists)
    heap_build(lists, size)
    for i in range(0, size)[::-1]:
        lists[0], lists[i] = lists[i],lists[0]
        heap_build(lists, i)
    return lists

input_list = [7,5,8,1,4,3,55,91,30]
output_list = []
output_list = heap_sort(input_list)
print(output_list)
```

    [1, 3, 4, 5, 7, 8, 30, 55, 91]



```python
"""
归并排序
通过递归实现将2个已经有序的子序列归并成一个有序的长序列
"""
def merge(left, right):
    i, j = 0, 0
    result = []
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result += left[i:]
    result += right[j:]
    return result

def  merge_sort(lists):
    if len(lists) <= 1:
        return lists
    num = len(lists) // 2
    left = merge_sort(lists[:num])
    right = merge_sort(lists[num:])
    return merge(left, right)

input_list = [7,5,8,1,4,3,55,91,30]
output_list = []
output_list = merge_sort(input_list)
print(output_list)
```

    [1, 3, 4, 5, 7, 8, 30, 55, 91]



```python
"""
基数排序
既桶排序，将要排序的元素分布到桶中，
"""
def bucket(lst):
    buckets = [0] * ((max(lst) - min(lst))+1)
    for i in range(len(lst)):
        buckets[lst[i]-min(lst)] += 1
    res=[]
    for i in range(len(buckets)):
        if buckets[i] != 0:
            res += [i+min(lst)]*buckets[i]
    return res

input_list = [7,5,8,1,4,3,55,91,30]
output_list = []
output_list = bucket_sort(input_list)
print(output_list)
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-18-0e1f9c1802cd> in <module>()
         15 input_list = [7,5,8,1,4,3,55,91,30]
         16 output_list = []
    ---> 17 output_list = bucket_sort(input_list)
         18 print(output_list)


    <ipython-input-13-fb2c83175c5d> in bucket_sort(lists, radix)
         10     for i in range(1, k+1):
         11         for j in lists:
    ---> 12             bucket[j / (radix **(i-1)) % (radix ** i)].append(j)
         13         del lists[:]
         14         for z in bucket:


    TypeError: list indices must be integers or slices, not float

