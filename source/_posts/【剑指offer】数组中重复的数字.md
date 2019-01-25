---
title: 【剑指offer】数组中重复的数字
date: 2019-01-18 20:33:20
tags:
- JavaScript
- 算法
categories:
- 剑指offer
---

## 找出数组中重复的数字

### 问题描述

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

###方法一

【思路】

- 从头到尾依次扫描这个数组中的每个数字
- 当扫描下标为i的数字时，首先比较这个数字(用m表示)是不是等于i
- 如果是，则接着扫描下一个数字，如果不是，则再拿它和第m个数字进行比较。
- 如果它和第m个数字相等，就找到一个重复的数字（该数字在下标i和m的位置都出现了）；
- 如果它和第m个数字不相等，就把第i个数字和第m个数字交换，把m放到属于它的位置
- 重复比较交换过程，直到我们发现一个重复的数字

```javascript
function duplicate(numbers, duplication)
{
    // write code here
    //这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    //函数返回True/False
    var len = numbers.length;
    for(let i=0; i<len; i++){
        while(numbers[i] !== i){   // 注意是while
            if(numbers[numbers[i]] == numbers[i]){
                duplication[0] = numbers[i];
                return true;
            } else {
                // 交换
                var temp = numbers[i];
                numbers[i] = numbers[temp];
                numbers[temp] = temp;
                //[numbers[numbers[i]], numbers[i]] = [numbers[i],numbers[numbers[i]]; ES6写法
            }
        }
    }
    return false;
}
```

> 时间复杂度为O（n），空间复杂度为O（1）
>
> 注意里面是while

###方法二

【思路】

题目里写了数组里数字的范围保证在0 ~ n-1 之间，所以可以利用现有数组设置标志，当一个数字被访问过后，可以设置对应位上的数 + n，之后再遇到相同的数时，会发现对应位上的数已经大于等于n了，那么直接返回这个数即可。

```javascript
function duplicate(numbers, duplication){
    var len = numbers.length;
    for(let i=0; i<len; i++){
        let index = numbers[i];
        //获得数组原本的数
        if(index >= len){
            index -= len;
        }
        if(numbers[index] >= len){
            duplication[0] = index;
            return true;
        }
        numbers[index] = numbers[index] + len;
    }
    return false;
}
```

## 不修改数组找出重复的数字

### 问题描述

在一个长度为n+1的数组里面的所有数字都在1~n的范围内，所以数组中至少有一个数字是重复的。请找出数组中任意一个重复的数字，但不能修改输入的数组。例如，如果输入长度为8的数组{2,3,5,4,3,2,6,7}，那么对应的输出是重复的数字2或者3。

### 方法一

【思路】

创建一个长度为n+1的辅助数组，逐一把原数组的数字复制到辅助数组，如将2存到arr[2]中， 3存到arr[3]中….，等下次遍历到3发现data[3]=3 说明重复了。

```javascript
function duplicate(numbers){
    var len = numbers.length;
    var arr = new Array(10).fill(0); // 创建长度为10，全为0的数组
    for(let i=0; i<len; i++){
        if(arr[numbers[i]] === 0){
            arr[numbers[i]] = numbers[i];
        } else {
            return numbers[i];
        }        
}
```

> 时间复杂度为 O(n)，空间复杂度O(n)

###方法二

【思路】

使用二分法。 如：{2,3,5,4,3,2,6,7} 先二分，先统计在1-4里面的数字个数如果大于4，则说明1-4里面有重复数字，否则5~7里面有重复数字。重复上面操作。

```javascript
function duplicate(numbers,duplication){
    var start = 1;
    var end = numbers.length-1;
    while(start <= end){        
        var middle = (end + start) >> 1; //取中
        var count = getCount(numbers,start,middle);
        if(start == end){
            if(count>1){
                //输出重复数字
                duplication[0] = index;
                return true;                
            }
            return false;
        }
        if(count > middle-start+1){
            end = middle;
        } else {
            start = middle + 1;
        }
    }
}
//查找数组中值位于start和end之间的元素个数
function getCount(array,start,end){
	var count = 0;
    for(int i=0; i<array.length; i++){
        if(array[i] >= start && array[i] <= end){
            count++;
        }
    }
    return count;
}
```

> 时间复杂度为 O(nlogn)，空间复杂度O(1)
>
> 这个方法**不能找出所有的重复元素**。