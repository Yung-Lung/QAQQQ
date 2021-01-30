###### tags: `Java` `Sorting`
# Sorting
## Quick Sort
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

public class RandomArray {
    int[] arrayToSort ;

    public RandomArray(int length){
        arrayToSort = new int[length];
        List<Integer> forArray = new ArrayList<Integer>();
        Random ran = new Random();
        int a;
        int n = 1;
        while (n<=length){
            forArray.add(n);
            n++;
        }
        n=0;
        while (n!=length){
            a = ran.nextInt(forArray.size());
            arrayToSort[n] = forArray.get(a);
            forArray.remove(a);
            n++;
        }
    }

    public static int[] quickSort(int[] toSort){
        // len is the length of array to sort
        int len = toSort.length;

        // pick a point to slice into two pieces
        int pivot = toSort[len/2];
        int lenOfSmall = 0;
        for(int value:toSort){
            if(value < pivot){
                lenOfSmall++;
            }
        }
        int[] theSmallArray = new int[lenOfSmall];
        int[] theBigArray = new int[len-lenOfSmall-1];
        int indexSmall = 0;
        int indexBig = 0;
        for(int value:toSort){
            if(value < pivot){
                theSmallArray[indexSmall] = value;
                indexSmall++;
            }
            else if(value > pivot){
                theBigArray[indexBig] = value;
                indexBig++;
            }
        }

        // check sort or slice
        theSmallArray = checkSlice(theSmallArray);
        theBigArray   = checkSlice(theBigArray);

        // combine
        int[] sortedArray = new int[len];
        for (int n = 0 ;n < len ; n++){
            if(n < lenOfSmall){
                sortedArray[n] = theSmallArray[n];
            }
            else if(n == lenOfSmall){
                sortedArray[n] = pivot;
            }
            else {
                sortedArray[n] = theBigArray[n-lenOfSmall-1];
            }
        }
        return sortedArray;
    }

    public static int[] checkSlice(int[] arr){
        int len = arr.length;
        if (len > 100){
            return quickSort(arr);
        }
        else {
            return sorting(arr);
        }
    }

    public static int[] sorting(int[] arr){
        int len = arr.length;
        int insert;
        int temp;
        for (int times=1;times<len;times++){
            insert = arr[times];
            for (int index=0;index<times;index++){
                if(arr[times] < arr[index]){
                    temp = arr[index];
                    arr[index] = arr[times];
                    arr[times] = temp;
                }
            }
        }
        return arr;
    }

    public int[] getArrayToSort(){
        return arrayToSort;
    }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int len = sc.nextInt();
        long time1,time2;
        RandomArray ar = new RandomArray(len);
        System.out.println(Arrays.toString(ar.getArrayToSort()));
        time1 = System.currentTimeMillis();
        System.out.println(Arrays.toString(quickSort(ar.getArrayToSort())));
        time2 = System.currentTimeMillis();
        System.out.println(time2-time1);
    }

}
```
## Merge Sort
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

public class MergeSort {

    public static int[] creatRandomArray(int length){
        int[] arrayToSort = new int[length];
        List<Integer> forArray = new ArrayList<Integer>();
        Random ran = new Random();
        int a;
        int n = 1;
        while (n<=length){
            forArray.add(n);
            n++;
        }
        n=0;
        while (n!=length){
            a = ran.nextInt(forArray.size());
            arrayToSort[n] = forArray.get(a);
            forArray.remove(a);
            n++;
        }
        return arrayToSort;
    }

    public static int[] mergeSort(int[] toSort){
        // len is the length of array to sort
        int len = toSort.length;

        int[] leftArray = Arrays.copyOfRange(toSort,0,len/2);
        int[] rightArray = Arrays.copyOfRange(toSort,len/2,len);
        //System.out.println(Arrays.toString(leftArray));
        //System.out.println(Arrays.toString(rightArray));

        if (len > 3){
            leftArray = mergeSort(leftArray);
            rightArray = mergeSort(rightArray);
        }
        else if(len == 3){
            rightArray = mergeSort(rightArray);
        }
        int times = 0, indexLeft = 0, indexRight = 0;
        while (times < len){
            if(leftArray[indexLeft] < rightArray[indexRight]){
                toSort[times] = leftArray[indexLeft];
                indexLeft++;
            }
            else {
                toSort[times] = rightArray[indexRight];
                indexRight++;
            }
            times++;
            if(indexLeft == leftArray.length){
                for(;times<len;times++){
                    toSort[times] = rightArray[indexRight];
                    indexRight++;
                }
            }
            else if(indexRight == rightArray.length){
                for (;times<len;times++){
                    toSort[times] = leftArray[indexLeft];
                    indexLeft++;
                }
            }
        }

        return toSort;
    }



    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int len = sc.nextInt();
        long time1,time2;
        int[] randomArray = creatRandomArray(len);
        System.out.println(Arrays.toString(randomArray));
        time1 = System.currentTimeMillis();
        System.out.println(Arrays.toString(mergeSort(randomArray)));
        time2 = System.currentTimeMillis();
        System.out.println(time2-time1);
    }

}

```
## Heap Sort
```java
import java.util.ArrayList;
import java.util.List;
import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

public class HeapSort {

    public static int[] creatRandomArray(int length){
        int[] arrayToSort = new int[length+1];
        List<Integer> forArray = new ArrayList<Integer>();
        Random ran = new Random();
        int a;
        int n = 1;
        while (n<=length){
            forArray.add(n);
            n++;
        }
        n=1;
        while (n!=length+1){
            a = ran.nextInt(forArray.size());
            arrayToSort[n] = forArray.get(a);
            forArray.remove(a);
            n++;
        }
        return arrayToSort;
    }

    public static int[] heapSort(int[] arr){
        int[] sortedArray = new int[arr.length];
        arr = buildMaxHeapTree(arr);
        for (int index = arr.length-1;index!=0;index--){
            sortedArray[index] = arr[1];
            arr[1] = arr[index];
            arr[index] = 0;
            arr = maxHeapify(arr,1);
        }
        return sortedArray;
    }

    public static int[] maxHeapify(int[] arr, int index){
        int len = arr.length;
        int theMax = arr[index];
        int theLeft  = arr[index*2];
        int theRight = 0;
        if(theLeft > theMax){
            theMax = theLeft;
        }
        if(len > index*2+1){
            theRight = arr[index*2+1];
            if (theRight > theMax){
                theMax = theRight;
                arr[index*2+1] = arr[index];
                arr[index] = theMax;
                if(len > 2*(index*2+1)){
                    arr = maxHeapify(arr,index*2+1);
                }
            }
            else if(theLeft == theMax){
                arr[index*2] = arr[index];
                arr[index] = theMax;
                if(len > 2*(index*2)){
                    arr = maxHeapify(arr,index*2);
                }
            }
            return arr;
        }
        else if(theLeft == theMax){
            arr[index*2] = arr[index];
            arr[index] = theMax;
        }
        return arr;
    }

    public static int[] buildMaxHeapTree(int[] arr){
        for(int i=(arr.length/2)-1 ; i!=0;i--){
            arr = maxHeapify(arr,i);
        }
        return arr;
    }

    public static void main(String[] args){
        Scanner sc = new Scanner(System.in);
        int len = sc.nextInt();
        long time1,time2;
        int[] randomArray = creatRandomArray(len);
        System.out.println(Arrays.toString(randomArray));
        time1 = System.currentTimeMillis();
        System.out.println(Arrays.toString(heapSort(randomArray)));
        time2 = System.currentTimeMillis();
        System.out.println(time2-time1);
    }

}

```
