---
title: 【剑指offer】替换空格
date: 2019-01-19 20:33:20
tags:
- JavaScript
- 算法
categories:
- 剑指offer
---

## 替换空格

### 问题描述

请实现一个函数，将一个字符串中的每个空格替换成“%20”。

例如，当字符串为We Are Happy。则经过替换之后的字符串为We%20Are%20Happy。

### JS自带方法

正则
```javascript
function replaceSpace(str){
    var reg = new RegExp(" ","g");  //g表示执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）
    return str.replace(reg,"%20");
}
```
正则
```javascript
function replaceSpace(str){
    var exp = /\s/g;   // \s表示匹配任何空白字符，包括空格、制表符、换页符等等
    return str.replace(exp,"%20");
}
```
split
```javascript
function replaceSpace(str){
　　return str.split(' ').join('%20');
}
```

### **从尾到头遍历字符串**

> js中不推荐此方法，主要借鉴算法的思路

【思路】

- 统计空格数，字符串长度增加——空格数 * 2
- 维持两个指针p1, p2
- p1指向原字符串长度末尾， p2指向新字符串长度末尾
- 当p1遇到空格时， p2 向前移动并替换字符为 '0' '2' '%'
- 把p1指针指向的字符拷贝到p2指针指向的字符
- 所有字符都只移动一次，时间复杂度为O(n)

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        // 空格的个数
        int spaceNum = 0;
        // 遍历查找空格的个数
        for(int i=0; i<str.length(); i++) {
            if(str.charAt(i) == ' ') {
                spaceNum ++;
            }
        }
        int indexOld = str.length() - 1;  // 原字符串下标
        int indexNew =  str.length() + 2 * spaceNum - 1; // 新字符串下标
        str.setLength(indexNew+1);   // 设置新的str长度
        while (indexOld >= 0 ) {
            if(str.charAt(indexOld) == ' ') {
                str.setCharAt(indexNew--, '0');
                str.setCharAt(indexNew--, '2');
                str.setCharAt(indexNew--, '%');
            } else {
                str.setCharAt(indexNew--, str.charAt(indexOld));
            }
            indexOld --;
        }
        return str.toString();
        
    } 
}
```

> JS字符串删除指定位置的字符 `newStr = str.substring(0,i) +  str.substring(i+1, str.length)`
>
> 注意是从右向左，不是从左向右



## 举一反三

在合并两个数组（包括字符串）时，如果从前向后复制每个数字（或字符）则需要重复移动数字（或字符）多次，那么我们可以考虑从后向前复制，这样就能减少移动的次数，从而提高效率。