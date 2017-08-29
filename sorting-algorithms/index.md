# 排序算法



## 插入排序—直接插入排序
时间复杂度(avg) | 时间复杂度(min) | 时间复杂度(max) | 空间复杂度 | 稳定度	
:----:|:----:|:----:|:----:|:----:
O(n2) | O(n) | O(n2) | O(1) |  稳定

> 插入排序法是将一个数目插入该占据的位置。从第二个数开始，取出这个数循环与更前的一个数比较，如果比它小就交换位置，比它大则取下一个数继续比较。（像整理扑克牌）

```js
function straightInsertionSort(dataList){
     var len = arr.length;
    var preIndex, current;
    for (var i = 1; i < len; i++) {
        preIndex = i - 1;
        current = arr[i];
        while(preIndex >= 0 && arr[preIndex] > current) {
            arr[preIndex+1] = arr[preIndex];
            preIndex--;
        }
        arr[preIndex+1] = current;
    }
    return arr;
}
```


## 插入排序—希尔排序
时间复杂度(avg) | 时间复杂度(min) | 时间复杂度(max) | 空间复杂度 | 稳定度	
:----:|:----:|:----:|:----:|:----:
O(n1.3) | O(n) | O(n2) | O(1) |  不稳定

> 希尔排序是将整个无序序列分割成若干小的子序列分别进行插入排序。序列分割方法：将相隔某个增量h的元素构成一个子序列.在排序过程中,逐次减小这个增量,最后当h减到1时,进行一次插入排序,排序就完成.增量序列一般采用：ht=2t-1,1≤t≤[log2 n],其中n为待排序序列的长度.

```js
function shellSort(arr) {
    var len = arr.length,
        temp,
        gap = 1;
    while (gap < len / 3) {  //动态定义间隔序列
        gap = gap * 3 + 1;
    }
    for (; gap > 0; gap = Math.floor(gap / 3)) {
        for (var i = gap; i < len; i++) {
            temp = arr[i];
            for (var j = i - gap; j > 0 && arr[j] > temp; j -= gap) {
                arr[j + gap] = arr[j];
            }
            arr[j + gap] = temp;
        }
    }
    return arr;
}
```


## 选择排序—简单选择排序
时间复杂度(avg) | 时间复杂度(min) | 时间复杂度(max) | 空间复杂度 | 稳定度	
:----:|:----:|:----:|:----:|:----:
O(n2) | O(n2) | O(n2) | O(1) |  不稳定

> 选择排序是一种简单直观的排序算法。它的工作原理是每一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，直到全部待排序的数据元素排完。

```js
function simpleSelectionSort(){
    var len = arr.length;
    var minIndex, temp;
    for (var i = 0; i < len - 1; i++) {
        minIndex = i;
        for (var j = i + 1; j < len; j++) {
            if (arr[j] < arr[minIndex]) {  //寻找最小的数
                minIndex = j;              //将最小数的索引保存
            }
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    return arr;
}
```


## 选择排序—堆排序
时间复杂度(avg) | 时间复杂度(min) | 时间复杂度(max) | 空间复杂度 | 稳定度	
:----:|:----:|:----:|:----:|:----:
O(n*log2 n) | O(n*log2 n) | O(n*log2 n) | O(1) |  不稳定

> 堆排序是一种利用堆的概念来排序的选择排序。分为两种方法：大顶堆：每个节点的值都大于或等于其子节点的值，在堆排序算法中用于升序排列；小顶堆：每个节点的值都小于或等于其子节点的值，在堆排序算法中用于降序排列

```js
var len;    //因为声明的多个函数都需要数据长度，所以把len设置成为全局变量

function buildMaxHeap(arr) {   //建立大顶堆
    len = arr.length;
    for (var i = Math.floor(len/2); i &gt;= 0; i--) {
        heapify(arr, i);
    }
}

function heapify(arr, i) {     //堆调整
    var left = 2 * i + 1,
        right = 2 * i + 2,
        largest = i;

    if (left < len && arr[left] > arr[largest]) {
        largest = left;
    }

    if (right < len && arr[right] > arr[largest]) {
        largest = right;
    }

    if (largest != i) {
        swap(arr, i, largest);
        heapify(arr, largest);
    }
}

function swap(arr, i, j) {
    var temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

function heapSort(arr) {
    buildMaxHeap(arr);

    for (var i = arr.length-1; i > 0; i--) {
        swap(arr, 0, i);
        len--;
        heapify(arr, 0);
    }
    return arr;
}
```

## 交换排序—冒泡排序
时间复杂度(avg) | 时间复杂度(min) | 时间复杂度(max) | 空间复杂度 | 稳定度	
:----:|:----:|:----:|:----:|:----:
O(n2) | O(n) | O(n2) | O(1) |  稳定

> 冒泡排序是一种计算机科学领域的较简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成

```js
function bubbleSort(arr) {
    var len = arr.length;
    for (var i = 0; i < len; i++) {
        for (var j = 0; j < len - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {        //相邻元素两两对比
                var temp = arr[j + 1];        //元素交换
                arr[j + 1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}
```



## 交换排序—快速排序
时间复杂度(avg) | 时间复杂度(min) | 时间复杂度(max) | 空间复杂度 | 稳定度	
:----:|:----:|:----:|:----:|:----:
O(n*log2 n) | O(n*log2 n) | O(n2) | O(n*log2 n)|  不稳定

> 快速排序是对冒泡排序的一种改进。通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

```js
function quickSort(arr, left, right) {
    var len = arr.length,
        partitionIndex,
        left = typeof left != 'number' ? 0 : left,
        right = typeof right != 'number' ? len - 1 : right;

    if (left < right) {
        partitionIndex = partition(arr, left, right);
        quickSort(arr, left, partitionIndex - 1);
        quickSort(arr, partitionIndex + 1, right);
    }
    return arr;
}

function partition(arr, left, right) {     //分区操作
    var pivot = left,                      //设定基准值（pivot）
        index = pivot + 1;
    for (var i = index; i <= right; i++) {
        if (arr[i] < arr[pivot]) {
            swap(arr, i, index);
            index++;
        }
    }
    swap(arr, pivot, index - 1);
    return index - 1;
}

function swap(arr, i, j) {
    var temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```


## 基数排序
时间复杂度(avg) | 时间复杂度(min) | 时间复杂度(max) | 空间复杂度 | 稳定度	
:----:|:----:|:----:|:----:|:----:
O(d(r+n)) | O(d(n+rd)) | O(d(r+n)) | O(d(rd+n)) |  稳定

> 基数排序 属于“分配式排序”（distribution sort），又称“桶子法”（bucket sort）或bin sort，顾名思义，它是透过键值的部份资讯，将要排序的元素分配至某些“桶”中，藉以达到排序的作用，基数排序法是属于稳定性的排序，其时间复杂度为O (nlog(r)m)，其中r为所采取的基数，而m为堆数，在某些时候，基数排序法的效率高于其它的稳定性排序法。有两种实现方法：最高位优先(Most Significant Digit first)法，简称MSD法；最低位优先(Least Significant Digit first)法，简称LSD法。

```js
//LSD Radix Sort
var counter = [];
function radixSort(arr, maxDigit) {
    var mod = 10;
    var dev = 1;
    for (var i = 0; i < maxDigit; i++ , dev *= 10, mod *= 10) {
        for (var j = 0; j < arr.length; j++) {
            var bucket = parseInt((arr[j] % mod) / dev);
            if (counter[bucket] == null) {
                counter[bucket] = [];
            }
            counter[bucket].push(arr[j]);
        }
        var pos = 0;
        for (var j = 0; j < counter.length; j++) {
            var value = null;
            if (counter[j] != null) {
                while ((value = counter[j].shift()) != null) {
                    arr[pos++] = value;
                }
            }
        }
    }
    return arr;
}
```
