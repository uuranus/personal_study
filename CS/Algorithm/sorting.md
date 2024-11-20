# Sort Algorithm
- 배열에 있는 데이터를 정렬하는 알고리즘

## Bubble Sort
- 인접한 두 데이터를 swap을 반복하면서 가장 큰 데이터를 sort되지 않은 부분의 제일 오른쪽으로 미는 알고리즘
- 데이터 상태에 상관없이 항상 O(n^2) 소요

```kotlin
fun bubbleSort(arr: IntArray){
    for(i in arr.size -1 downTo 1){
        for(j in 0 until i){
            if(arr[j] > arr[j+1]){
                swap(arr,j,j+1)
            }
        }
    }
}
```

## Insertion Sort
- 정렬이 되어있지 않은 부분에서 하나씩 정렬이 된 부분에 맞지 위치에 삽입(insertion)하는 정렬 알고리즘
- 정렬이 된 부분 마지막에서부터 한칸씩 비교하면서 자리를 찾아가는 방식이라 초기 데이터가 이미 정렬된 상태라면 O(n)이 걸리지만 역순이라면 O(n^2)가 소요될 수 있음

```kotlin
fun insertionSort(arr: IntArray){
    for(i in 1 until arr.size){
        var j = i-1
        while(j >= 0 && arr[j] > arr[j+1]){
            swap(arr,j,j+1) //삽입할 자리를 만들기 위해 한칸씩 뒤로
            j--
        }
    }
}
```

## Selection Sort
- 정렬이 되어있지 않은 부분에서 최솟값을 찾아서 정렬이 된 부분 뒤에 붙이는 정렬 알고리즘
- 데이터 상태에 상관없이 항상 O(n^2) 소요 (최솟값 찾기 O(n) * 이를 n번 반복)

```kotlin
fun selectionSort(arr: IntArray){
    
    for(i in 0 until arr.size){
        var min = arr[i]
        var minIndex = i
        for(j in i+1 until arr.size){
            if(min > arr[j]){
                min = arr[j]
                minIndex = j
            }
        }
        swap(arr,i,minIndex)
    }
}
```

## Merge Sort
- 정렬을 두 부분으로 나누어서 재귀적으로 나눈 부분을 정렬한 후 정렬된 두 부분을 합쳐나가는 정렬 알고리즘
- 데이터 상태에 상관없이 항상 O(nlogn) 소요 (데이터 반으로 쪼개기 O(logn) 쪼갤 때마다 합치는 과정 O(n))

```kotlin
fun mergeSort(arr: IntArray, start: Int, end: Int){

    if(end - start <= 1) return  

    val mid = start + (end-start) / 2

    mergeSort(arr, start, mid)
    mergeSort(arr, mid, end)

    val temp = IntArray(arr.size)
    var leftIndex = start
    var rightIndex = mid
    var tempIndex = start

    while(leftIndex < mid || rightIndex < end){
        if(leftIndex >= mid){
            temp[tempIndex] = arr[rightIndex]
            rightIndex++
        }
        else if(rightIndex >= end){
            temp[tempIndex] = arr[leftIndex]
            leftIndex++
        }
        else {
            if(arr[leftIndex] <= arr[rightIndex]){
                temp[tempIndex] = arr[leftIndex]
                leftIndex++
            }
            else {
                temp[tempIndex] = arr[rightIndex]
                rightIndex++
            }
        }
        tempIndex++
    }

    for(i in start until end){
        arr[i] = temp[i]
    }
}
```


## Quick Sort
- pivot을 기준으로 양쪽으로 pivot보다 작은값, 큰값으로 나누고 재귀적으로 양쪽 partition을 sort하는 정렬 알고리즘
- pivot에 따라서 O(nlogn)이 될 수도 있고 O(n^2)이 될 수 있으나 평균적으로 O(nlogn)이 소요 (극단에 있는 pivot을 고리면 partition한쪽으로 계속 쏠림)

```kotlin
fun quickSort(arr: IntArray, start: Int, end: Int){

    if (start >= end) return
    
    val pivot = arr[start]

    var left = start + 1 
    var right = end - 1

    while(left <= right){
        while(left<=right && arr[left] < pivot) left++
        while(left<=right && arr[right] >= pivot) right-- //swap 해야하는 지점까지 이동

        swap(arr,left, right)
        left++
        right--
    }

    quickSort(arr, start, left)
    quickSort(arr, left+1, end)
}
```

## Heap Sort
- 최대 Heap을 구현한 배열을 통해 정렬하는 알고리즘
- O(nlogn)소요 (배열을 힙으로 바꾸기 O(nlogn) + 최대값 추출 후 heapDown O(logn) * n번 반복 )

```kotlin
fun heapSort(arr: IntArray){

    val heap = heapify(arr) //최대 힙
    
    for(i in arr.size-1 downTo 0){
        swap(1,i)
        heapDown(arr, 1,i-1)
    }
}

fun heapify(arr: IntArray): IntArray {
    val n = arr.size
    for (i in n / 2 - 1 downTo 0) {
        heapDown(arr, i, n - 1)
    }
    return arr
}

fun heapDown(arr: IntArray, start:Int, end: Int){
    var i = start
    while(i <= end/2){
        val leftChild = i * 2
        val rightChild = i * 2 + 1

        val max = if(arr[leftChild] < rightChild) rightChild else leftChild

        if(i < arr[max]){
            swap(arr,i, max)
            i = max
        }
        else {
            break
        }
    }
}
```


**swap**
```kotlin
    fun swap(arr: IntArray, i: Int, j: Int) {
        val temp = arr[i]
        arr[i] = arr[j]
        arr[j] = temp
    }
```

## 정리

| Algorithm      | Best Case    | Average Case | Worst Case   | Stable |
|----------------|--------------|--------------|--------------|--------|
| Bubble Sort    | O(n)         | O(n²)        | O(n²)        | Yes    |
| Insertion Sort | O(n)         | O(n²)        | O(n²)        | Yes    |
| Selection Sort | O(n²)        | O(n²)        | O(n²)        | No     |
| Merge Sort     | O(n log n)   | O(n log n)   | O(n log n)   | Yes    |
| Quick Sort     | O(n log n)   | O(n log n)   | O(n²)        | No     |
| Heap Sort      | O(n log n)   | O(n
