---
title: 【剑指offer】二维数组的查找
date: 2019-01-19 20:33:20
tags:
- JavaScript
- 算法
categories:
- 剑指offer
---

## 二维数组的查找

### 问题描述

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

###方法一

【思路】

从**左下角（或者右上角）**开始遍历数组，如果当前的元素大于target，则说明目标元素在当前元素的右边；如果当前的元素小于target，则说明目标元素在当前元素的上边。

```javascript
function Find(target, array)
{
    var row = array.length, col = array[0].length; 
    var i = 0, j = col-1;
    while(i < row && j >= 0){
        if(target > array[i][j]){
            i++;
        }else if(target < array[i][j]){
            j--;
        }else{
            return true;
        }
    }
    return false;
}
```

> 时间复杂度为O（n），空间复杂度为O（1）
>
> 不能选择左上角或右下角开始
