## Q: js实现排序算法

## A:

### 冒泡排序
```javascript
const bubbleSort = arr => {
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = 0; j < arr.length - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]
      }
    }
  }
}
```

### 插入排序
```javascript
const insertSort = arr => {
  for(let i = 1; i < arr.length; i++) {
    for (let j = 0; j < i; j++) {
      if (arr[j] > arr[i]) {
        arr.splice(j, 0, arr[i])
        arr.splice(i + 1, 1)
      }
    }
  }
}
```

### 归并排序
```javascript
const merge = (left, right) => {
  const result = []
  while (left.length > 0 && right.length >0) {
    if (left[0] < right[0]) {
      result.push(left.shift())
    } else {
      result.push(right.shift())
    }
  }
  return result.contact(left, right)
}

const mergeSort = arr => {
  if (arr.length <= 1) return arr
  const middle = Math.floor(arr.length / 2)
  const left = arr.slice(0, middle)
  const right = arr.slice(middle)
  return merge(mergeSort(left), mergeSort(right))
}  
```

### 选择排序
```javascript
const selectSort = arr => {
  for (let i = 0; i < arr.length - 1; i++) {
    let min = i
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[min]) min = j
    }
    [arr[i], arr[min]] = [arr[min], arr[j]]
  }
}
```

### 快速排序
```javascript
const quickSort = arr => {
  if (arr.length <= 1) return arr
  const pivot = arr[0]
  const left = []
  const right = []
  for (let i = 1; i < arr.length; i++) {
    arr[i] < pivot ? left.push(arr[i]) : right.push(arr[i])
  }
  return quickSort(left).concat(pivot, quickSort(right))
}
```