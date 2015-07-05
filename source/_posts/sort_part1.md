title: 排序算法----基于比较的排序
date: 2015-07-04 14:18:53
categories: 数据结构和算法
tags: 算法
---
###插入排序
插入排序的原理很简单很简单，打扑克牌时理牌的思想就是插入排序。

    void InsertionSort(int A[], int n){  //对大小为n的整型数组进行排序
      int i,j;
      int key;
      //从第二个元素开始，边比较边移动 
      for(i=1;i<n;i++){
        j=i-1;
        key=A[i];
        while(j>=0 && A[j]>key){
        A[j+1]=A[j];
          j--;
        }
        A[j+1]=key;
      }    
    }   
插入排序算法的时间复杂度为O(N^2)，空间复杂度为O(1)，排序稳定。
###归并排序
归并排序利用了分治的思想，先把待排序数组分成两个字数组，子数组排好序后，合并成一个排好序的大数组，而子数组的排序可以递归的调用归并排序算法本身。  

归并排序的关键在于归并过程： 

    void Merge(int A[], int p, int q, int r){
      int n1=q-p+1;
      int n2=r-q;
      int i,j,k;
      //创建两个数组，分别保存待合并的左边和右边的数组。
      int *arr1=(int *)malloc((n1+1)*sizeof(int));
      int *arr2=(int *)malloc((n2+1)*sizeof(int));
      //复制原数组到元素到新创建的数组。
      for(i=0;i<n1;i++)  arr1[i]=A[p+i];
      for(i=0;i<n2;i++)  arr2[i]=A[q+1+i];
      //这里关键处，设置了两个"哨兵牌"，简化了后面的归并程。
      arr1[n1]=INT_MAX;
      arr2[n2]=INT_MAX;
      //归并过程，由于有"哨兵"的存在，不需要判断i,j是否会分别大于n1,n2。
      i=j=0;
      for(k=p;k<=r;k++){
        if(arr1[i]<=arr2[j]){
          A[k]=arr1[i];
          i++;
        }
        else{
          A[k]=arr2[j];
          j++;
        }     
      } 
    }
    
利用归并过程对数组进行归并排序：

    void merge_sort(int A[], int low, int high){
      if(low<high){
         int mid=(high+low)/2;
         merge_sort(A,low,mid);
         merge_sort(A,mid+1,high);
         Merge(A,low,mid,high);
      }
    }
    
归并排序函数：

    void MergeSort(int A[], int n){
      merge_sort(A,0,n-1);
    }
归并排序算法的时间复杂度为O(N*logN),空间复杂度为O(N),排序稳定。
###堆排序
归并排序的时间复杂度比插入排序小，但是空间复杂度大，堆排序结合了两者的优点。  
堆排序使用一种叫做(二叉)堆的数据结构，其可以被视作一颗完全二叉树。堆排序的主要过程为保持堆的性质。假设一棵完全二叉树除了根节点之外，其左右子树都是大顶堆，则保持整棵树为大顶堆的过程keep_max_heap为：
    
    //以i为根的二叉数的左右子树都为大顶堆。
    //这个过程使以i为根的整棵树成为大顶堆。    
    void keep_max_heap(int A[], int i, int heap_size, int n){ 
      int l=2*i; //左子树的根
      int r=2*i+1; //右子树的根
      //找出i,l,r值最大的结点
      int temp,largest=i;
      if(l<heap_size && A[l]>A[i]){
        largest=l;
      }
      if(r<heap_size && A[r]>A[largest]){
        largest=r;
      }
      //若根非最大结点，将根的值与最大结点对换，然后递归调用自身保持子树的最大堆性质
      if(largest!=i){
        emp=A[i];
        A[i]=A[largest];
        A[largest]=temp;
        keep_max_heap(A,largest,heap_size,n);
      }
    }

上述保持堆的性质的过程的时间复杂度为O(h)，h为树的高度。  
接下来利用keep_max_heap过程将一个数组构建成最大堆：
    
    void build_max_heap(int A[],int n){
      int i;
      int heap_size=n;
      //从第最后一个非叶结点开始，依次构建调用keep_max_heap过程
      //可以证明，n/2为最后一个非叶节点
      for(i=n/2-1;i>=0;i--)  
        keep_max_heap(A,i,heap_size,n);
    }
    
build_max_heap过程的时间复杂度为O(N)，详细的证明过程可以参考《算法导论》一书。  
堆排序函数：

    void HeapSort(int A[], int n){
      int heap_size=n;
      build_max_heap(A,n);
      int temp;
      while(heap_size>1){
        temp=A[0];
        A[0]=A[heap_size-1];
        A[heap_size-1]=temp;
        keep_max_heap(A,0,heap_size-1,n);
        heap_size--;
      }
    }
    
堆排序的时间复杂度为O(N*logN),空间复杂度为O(1),排序不稳定。
###快速排序
快速排序是实际使用中非常重要的一种排序算法，它最坏情况下的时间复杂度为O(N^2)，但是平均时间复杂度为O(N*logN),快速排序的一种好的实现比堆排序和归并排序都要快。      
快速排序也是基于分治的思想，先将整个数组用一个元素划分，使该元素左边的部分都比它小，使右边的部分都比它大，然后对左右两边的数组递归调用快速排序自身。快速最重要的部分是划分过程：

    int Partion(int A[], int left, int right){
      int center=(left+right)/2;
      if (A[left]>A[center])  swap(&A[left],&A[center]);
      if (A[left]>A[right])   swap(&A[left],&A[right]);
      if (A[center]>A[right]) swap(&A[center],&A[right]);
      swap(&A[center],&A[right-1]);
      return A[right-1];
    }
    
快速排序过程：
    
    void quick_sort(int A[], int left, int right){
      int i,j,mid;
      if(left+10<right){  //数组元素个数较大时使用快排
        mid=Partion(A,left,right);
        i=left;
        j=right-1;
        for(;;){
          //从左往右找到第一个不小于mid的元素
          //A[right-1]==mid保证了寻找过程不会越界
          while(A[++i]<mid) {}  
          //从右往左找到第一个不大于mid的元素
          //A[left]<=mid保证了寻找过程不会数组越界
          while(A[--j]>mid) {}  
          //交换
          if(i<j) swap(&A[i],&A[j]);
          else break;
        }
        //把A[right-1]处的值交换到适当的位置
        swap(&A[i],&A[right-1]);
        //对子数组调用自身排序
        quick_sort(A,left,i-1);
        quick_sort(A,i+1,right);
      }
      else  //数组元素较小，使用插入排序
      InsertionSort(A+left,right-left+1);
    }
    
快速排序函数：

    void QuickSort(int A[], int n){
      quick_sort(A,0,n-1);
    }
    
快速排序其平均时间复杂度为O(N\*logN),空间复杂度为O(1),由于其内部循环非常紧凑，所以运行速度比堆排序和归并排序都要快。这里的排序过程对有序数组和数据全部相同的数组排序时的时间复杂度都是O(N\*logN)。另外当数组比较小时，采用了插入排序，进一步提升了其性能。     