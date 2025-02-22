---
layout: post
title: '冒泡排序也可以这么玩'
date: 2019-11-18 20:00:21 +0800
author: ma_meng
color: rgb(135,206,250)
cover: 'https://img-blog.csdnimg.cn/20191118210734714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2d1b2thaWdkZw==,size_16,color_FFFFFF,t_70'
subtitle: '没错这又是面试题目'
tags: 面试题
---

> 面试遇到一个算法题, 其中有个排序用冒泡解决的, 然后问还能优化吗, 一开始就想出一个缩短比对范围的, 别的想不起来了, 之后就一脸蒙蔽 好吧现在来讨论下, 时间复杂度为O(n²) 还能怎么优化 :

### 1. 如果本来就是有序的, 判断下之后就不用再排序了, 就这么简单:

```js
  var flag = false;   //标志位
  function bubblesort(a){
      if(a.length < 1) return;
      for(let i = 0;i < a.length;i++){
        for(let j = 0;j < a.length-1-i;j++){
            if(a[j]>=a[j+1]){
                let temp = a[j+1];
                a[j+1] = a[j];
                  a[j] = temp;
                  flag = true;
              }
          }
          if(!flag){ //第一次循环如果没有位置交换, 说明原来就是有序的,直接返回原来数组就完事
              break;
          }
        }
      return a;
    }
```
### 2.设置一标志性变量pos,用于记录每趟排序中最后一次进行交换的位置。由于pos位置之后的记录均已交换到位,故在进行下一趟排序时只要扫描到pos位置即可
```js
function bubbleSort2(arr) {
    console.time('改进后冒泡排序耗时');
    var i = arr.length-1;  //初始时,最后位置保持不变
    while ( i> 0) {
        var pos= 0; //每趟开始时,无记录交换
        for (var j= 0; j< i; j++)
            if (arr[j]> arr[j+1]) {
                pos= j; //记录交换的位置
                var tmp = arr[j]; arr[j]=arr[j+1];arr[j+1]=tmp;
            }
        i= pos; //为下一趟排序作准备
     }
     console.timeEnd('改进后冒泡排序耗时');
     return arr;
}
```

### 3.传统冒泡排序中每一趟排序操作只能找到一个最大值或最小值,我们考虑利用在每趟排序中进行正向和反向两遍冒泡的方法一次可以得到两个最终值(最大者和最小者) , 从而使排序趟数几乎减少了一半。

```js
function bubbleSort3(arr) {
    var low = 0;
    var high= arr.length-1; //设置变量的初始值
    var tmp,j;
    console.time('2.改进后冒泡排序耗时');
    while (low < high) {
        for (j= low; j< high; ++j) //正向冒泡,找到最大者
            if (arr[j]> arr[j+1]) {
                tmp = arr[j]; arr[j]=arr[j+1];arr[j+1]=tmp;
            }
        --high;                 //修改high值, 前移一位
        for (j=high; j>low; --j) //反向冒泡,找到最小者
            if (arr[j]<arr[j-1]) {
                tmp = arr[j]; arr[j]=arr[j-1];arr[j-1]=tmp;
            }
        ++low;                  //修改low值,后移一位
    }
    console.timeEnd('2.改进后冒泡排序耗时');
    return arr;
}
```
