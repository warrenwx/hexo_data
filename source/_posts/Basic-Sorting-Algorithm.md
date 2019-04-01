---
title: Basic-Sorting-Algorithm
date: 2018-08-22 20:41:14
tags:
---
0.1.2.?2. quicksort

```cpp
/*
  input: 
          arr:  array for type int *
          low:  index of first element in array
          high: index of last element in arrray
  output:
          arr
  return:
          the last pivotkey index
  prerequisites:
          none 
*/
int partition(int arr[], int low, int high){

    int pivotkey = arr[low];
    int index = 0;
    int temp = 0;

    while(low < high){
        while(low < high && arr[high] >= pivotkey) high--;
        temp = arr[low];
        arr[low] = arr[high];
        arr[high] = temp;

        while(low < high && arr[low] <= pivotkey) low++;
        temp = arr[low];
        arr[low] = arr[high];
        arr[high] = temp;
    }
    arr[low] = pivotkey;

    for(index = 0; index <= 5; index++){
        printf("%d\t", arr[index]);
    }
    printf("\n");
    return low;
}

void qsort(int arr[], int low, int high){

    if(low < high){
        int retIndex = partition(arr, low, high);
        qsort(arr, low, retIndex - 1);
        qsort(arr, retIndex + 1, high);
    }
}
```