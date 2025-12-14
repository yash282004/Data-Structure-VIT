# Program by Yash Santosh Khandagale

## Problem Statement / Title  
Write a program to input marks of **n students**, sort the marks in ascending order using the **Quick Sort algorithm** (without built-in functions) and analyse the sorting algorithm **pass by pass**. Find the **minimum and maximum marks** using **Divide and Conquer (recursively)**.

## Description  
We input marks of `n` students into an array.  
Then:  
- We sort using **Quick Sort** (our own implementation, not built-in).  
- After each partition, we print the array (to show **pass by pass** progress).  
- Finally, we use **Divide and Conquer recursively** to find **minimum** and **maximum** marks efficiently.

## Code  
```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

void quickSort(int* arr,int low,int high){
    if(low<high){
        int pivot=arr[high];
        int i=low-1;
        for(int j=low;j<high;j++){
            if(arr[j]<pivot){
                i++;
                int temp=arr[i];
                arr[i]=arr[j];
                arr[j]=temp;
            }
        }
        int temp=arr[i+1];
        arr[i+1]=arr[high];
        arr[high]=temp;

        cout<<"After pass with pivot "<<pivot<<": ";
        for(int k=0;k<high+1;k++)
            cout<<arr[k]<<" ";
        cout<<endl;

        int pi=i+1;
        quickSort(arr,low,pi-1);
        quickSort(arr,pi+1,high);
    }
}

void findMinMax(int* arr,int low,int high,int &minVal,int &maxVal){
    if(low==high){
        minVal=arr[low];
        maxVal=arr[low];
        return;
    }
    if(high==low+1){
        if(arr[low]<arr[high]){
            minVal=arr[low];
            maxVal=arr[high];
        }else{
            minVal=arr[high];
            maxVal=arr[low];
        }
        return;
    }
    int mid=(low+high)/2;
    int min1,max1,min2,max2;
    findMinMax(arr,low,mid,min1,max1);
    findMinMax(arr,mid+1,high,min2,max2);
    minVal=(min1<min2)?min1:min2;
    maxVal=(max1>max2)?max1:max2;
}

int main(){
    srand(time(0));
    int n;
    cout<<"Enter number of students:";
    cin>>n;
    int* marks=new int[n];
    cout<<"Generated marks of students:\n";
    for(int i=0;i<n;i++){
        marks[i]=rand()%101;
        cout<<marks[i]<<" ";
    }
    cout<<endl;

    quickSort(marks,0,n-1);

    cout<<"\nSorted Marks: ";
    for(int i=0;i<n;i++)
        cout<<marks[i]<<" ";
    cout<<endl;

    int minMarks,maxMarks;
    findMinMax(marks,0,n-1,minMarks,maxMarks);
    cout<<"Minimum Marks: "<<minMarks<<endl;
    cout<<"Maximum Marks: "<<maxMarks<<endl;

    delete[] marks;
    return 0;
}
```

## Example Output  
```
Enter number of students:10
Generated marks of students:
32 98 71 1 96 76 5 35 41 7 
After pass with pivot 7: 1 5 7 32 96 76 98 35 41 71 
After pass with pivot 5: 1 5 
After pass with pivot 71: 1 5 7 32 35 41 71 96 76 98 
After pass with pivot 41: 1 5 7 32 35 41 
After pass with pivot 35: 1 5 7 32 35 
After pass with pivot 98: 1 5 7 32 35 41 71 96 76 98 
After pass with pivot 76: 1 5 7 32 35 41 71 76 96 

Sorted Marks: 1 5 7 32 35 41 71 76 96 98 
Minimum Marks: 1
Maximum Marks: 98
```
