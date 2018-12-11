# 二分查找

以下内容参考刘汝佳的白皮书。 

在有序表中查找元素常常使用二分查找(Binary Search)，时间复杂度为O(logn)。 

一般的算法流程为：待查找的值与数组中间值作比较，如果前者小于后者则在左子数组中继续查找；如果大于则在右子数组中继续查找；如果等于则查找结束。

写出一个能查找到所求值`target`所在的位置的二分查找程序并不难，难的是怎么样是一个好的程序？ 如果数组中存在多个`target`，我们该返回哪个`target`呢？接下来我们会给出两个二分查找的程序，相信能解决大多数的情况，可以作为二分查找的一种标准写法。

## 返回target第一次出现的下标

下面给一个这样的程序，当`target`存在时返回它出现的第一个位置，如果不存在，则返回这样一个下标 `i`：在此处插入`target`，后序列依然有序。 

```python
def binary_search(vals, target):
    low, high = 0, len(vals)
    while low < high:
        mid = low + (high - low) // 2  # 防止溢出
        if target > vals[mid]:
            low = mid + 1
        else:
            high = mid
    return low

if __name__ == '__main__':

    vals = [5,5,9,10,12]
    print(binary_search(vals, 5))  # 0
    print(binary_search(vals, 8))  # 2
    print(binary_search(vals, 13))  # 5
```

来分析一下这个程序，首先最后的返回值不仅可能是 0 ,1, 2, ....., n-1,还可能是n(如果`target`大于`vals[n-1]`)。所以返回值的候选区间是`[0,n]`。 

`mid`的计算为什么用`low + (high - low) // 2`而不是`(low + high) // 2`？是为了防溢出，其实两式是一致的，证明如下 

```python
(left+right)//2 => (2*left+right-left)//2 => left + (right-left)//2
```



## 返回target最后一次出现的下标

那么求上界呢？如何得到值为`target`出现的最后一个位置呢？

```Python
def binary_search(vals, target):
    ow, high = 0, len(vals)
    while low < high:
        mid = low + (high - low) // 2  # 防止溢出
        if target >= vals[mid]:
            low = mid + 1
        else:
            high = mid
    return low

if __name__ == '__main__':

    vals = [5,5,9,10,12]
    print(binary_search(vals, 5))  # 2
    print(binary_search(vals, 8))  # 2
    print(binary_search(vals, 13))  # 5
```

观察可知，只是修改了一个地方，功能就完全不同。 

这里的二分查找返回的是`target`出现的最后一个位置的后面一个位置，如果不存在，则返回这样一个下标`i`：在此处插入`target`，后序列依然有序。这里要注意的是：**是target出现最后一个位置的后一个位置**。 