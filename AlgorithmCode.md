# 算法代码

## 排序类

### 冒泡排序

```java
package com.sort;

  //稳定
  public class 冒泡排序 {
      public static void main(String[] args) {
          int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
          System.out.println("排序之前：");
          for (int i = 0; i < a.length; i++) {
              System.out.print(a[i]+" ");
         }
         //冒泡排序
         for (int i = 0; i < a.length; i++) {
             for(int j = 0; j<a.length-i-1; j++){
                 //这里-i主要是每遍历一次都把最大的i个数沉到最底下去了，没有必要再替换了
                if(a[j]>a[j+1]){
                     int temp = a[j];
                     a[j] = a[j+1];
                     a[j+1] = temp;
                 }
             }
         }
         System.out.println();
         System.out.println("排序之后：");
         for (int i = 0; i < a.length; i++) {
             System.out.print(a[i]+" ");
         }
     }
 }
```



### 快速排序

```java
package com.sort;

//不稳定
public class 快速排序 {
    public static void main(String[] args) {
        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
        System.out.println("排序之前：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
        //快速排序
        quick(a);
        System.out.println();
        System.out.println("排序之后：");
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i]+" ");
        }
    }

    private static void quick(int[] a) {
        if(a.length>0){
            quickSort(a,0,a.length-1);
        }
    }

    private static void quickSort(int[] a, int low, int high) {
        if(low<high){ //如果不加这个判断递归会无法退出导致堆栈溢出异常
            int middle = getMiddle(a,low,high);
            quickSort(a, 0, middle-1);
            quickSort(a, middle+1, high);
        }
    }

    private static int getMiddle(int[] a, int low, int high) {
        int temp = a[low];//基准元素
        while(low<high){
            //找到比基准元素小的元素位置
            while(low<high && a[high]>=temp){
                high--;
            }
            a[low] = a[high]; 
            while(low<high && a[low]<=temp){
                low++;
            }
            a[high] = a[low];
        }
        a[low] = temp;
        return low;
    }
}
```





**1、基本思想:**

归并（Merge）排序法是将两个（或两个以上）有序表合并成一个新的有序表，即把待排序序列分为若干个子序列，每个子序列是有序的。然后再把有序子序列合并为整体有序序列。

### 归并排序

```java
package com.sort;

  //稳定
  public class 归并排序 {
      public static void main(String[] args) {
         int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
          System.out.println("排序之前：");
          for (int i = 0; i < a.length; i++) {
              System.out.print(a[i]+" ");
         }
         //归并排序
         mergeSort(a,0,a.length-1);
         System.out.println();
         System.out.println("排序之后：");
         for (int i = 0; i < a.length; i++) {
             System.out.print(a[i]+" ");
         }
     }

     private static void mergeSort(int[] a, int left, int right) {
         if(left<right){
             int middle = (left+right)/2;
             //对左边进行递归
             mergeSort(a, left, middle);
             //对右边进行递归
             mergeSort(a, middle+1, right);
             //合并
             merge(a,left,middle,right);
         }
     }

     private static void merge(int[] a, int left, int middle, int right) {
         int[] tmpArr = new int[a.length];
         int mid = middle+1; //右边的起始位置
         int tmp = left;
         int third = left;
         while(left<=middle && mid<=right){
             //从两个数组中选取较小的数放入中间数组
             if(a[left]<=a[mid]){
                 tmpArr[third++] = a[left++];
             }else{
                 tmpArr[third++] = a[mid++];
             }
         }
         //将剩余的部分放入中间数组
         while(left<=middle){
             tmpArr[third++] = a[left++];
         }
         while(mid<=right){
             tmpArr[third++] = a[mid++];
         }
         //将中间数组复制回原数组
         while(tmp<=right){
             a[tmp] = tmpArr[tmp++];
         }
     }
 }
```



### 基数排序

```java
package com.sort;

  import java.util.ArrayList;
  import java.util.List;
  //稳定
  public class 基数排序 {
      public static void main(String[] args) {
          int[] a={49,38,65,97,176,213,227,49,78,34,12,164,11,18,1};
          System.out.println("排序之前：");
         for (int i = 0; i < a.length; i++) {
             System.out.print(a[i]+" ");
         }
         //基数排序
         sort(a);
         System.out.println();
         System.out.println("排序之后：");
         for (int i = 0; i < a.length; i++) {
             System.out.print(a[i]+" ");
         }
     }

     private static void sort(int[] array) {
         //找到最大数，确定要排序几趟
         int max = 0;
         for (int i = 0; i < array.length; i++) {
             if(max<array[i]){
                 max = array[i];
             }
         }
         //判断位数
         int times = 0;
         while(max>0){
            max = max/10;
             times++;
         }
         //建立十个队列
         List<ArrayList> queue = new ArrayList<ArrayList>();
        for (int i = 0; i < 10; i++) {
            ArrayList queue1 = new ArrayList();
             queue.add(queue1);
       }
         //进行times次分配和收集
         for (int i = 0; i < times; i++) {
             //分配
             for (int j = 0; j < array.length; j++) {
                 int x = array[j]%(int)Math.pow(10, i+1)/(int)Math.pow(10, i);
                 ArrayList queue2 = queue.get(x);
                 queue2.add(array[j]);
                 queue.set(x,queue2);
             }
             //收集
             int count = 0;
             for (int j = 0; j < 10; j++) {
                 while(queue.get(j).size()>0){
                     ArrayList<Integer> queue3 = queue.get(j);
                     array[count] = queue3.get(0);
                     queue3.remove(0);
                     count++;
                 }
             }
         }
     }
 }
```

### 排序算法优劣性分析

#### **一、稳定性:**

稳定：冒泡排序、插入排序、归并排序和基数排序

不稳定：选择排序、快速排序、希尔排序、堆排序

#### **二、平均时间复杂度**

O(n^2):直接插入排序，简单选择排序，冒泡排序。

在数据规模较小时（9W内），直接插入排序，简单选择排序差不多。当数据较大时，冒泡排序算法的时间代价最高。性能为O(n^2)的算法基本上是相邻元素进行比较，基本上都是稳定的。

O(nlogn):快速排序，归并排序，希尔排序，堆排序。

其中，快排是最好的， 其次是归并和希尔，堆排序在数据量很大时效果明显。

#### **三、排序算法的选择**

　　1.数据规模较小

 　　（1）待排序列基本序的情况下，可以选择直接插入排序；

 　　（2）对稳定性不作要求宜用简单选择排序，对稳定性有要求宜用插入或冒泡

　　2.数据规模不是很大

　　（1）完全可以用内存空间，序列杂乱无序，对稳定性没有要求，快速排序，此时要付出log（N）的额外空间。

　　（2）序列本身可能有序，对稳定性有要求，空间允许下，宜用归并排序

　　3.数据规模很大

  　　（1）对稳定性有求，则可考虑归并排序。

  　　（2）对稳定性没要求，宜用堆排序

　　4.序列初始基本有序（正序），宜用直接插入，冒泡

