# Program by Yash Santosh Khandagale

## Problem Statement / Title  
Write a program to implement **Bubble Sort** and **Quick Sort** on a 1D array of Student structure (contains student_name, student_roll_no, total_marks), with key as **student_roll_no**. Also count the number of swaps performed by each method.

## Description  
We have a list of students (name, roll number, total marks).  
We’ll sort them:
- Using **Bubble Sort** (simple but slow for large data).
- Using **Quick Sort** (faster, divide & conquer).  

We’ll also count how many **swaps** are performed by each sorting algorithm.

## Code  
```cpp
#include <iostream>
#include <stdlib.h>
using namespace std;

struct Student{
    string student_name;
    int student_roll_no;
    int total_marks;
};

void bubbleSort(Student* arr,int n,int &swapCount){
    swapCount=0;
    for(int i=0;i<n-1;i++){
        for(int j=0;j<n-i-1;j++){
            if(arr[j].student_roll_no>arr[j+1].student_roll_no){
                Student temp=arr[j];
                arr[j]=arr[j+1];
                arr[j+1]=temp;
                swapCount++;
            }
        }
    }
}

int partition(Student* arr,int low,int high,int &swapCount){
    int pivot=arr[high].student_roll_no;
    int i=low-1;
    for(int j=low;j<high;j++){
        if(arr[j].student_roll_no<pivot){
            i++;
            Student temp=arr[i];
            arr[i]=arr[j];
            arr[j]=temp;
            swapCount++;
        }
    }
    Student temp=arr[i+1];
    arr[i+1]=arr[high];
    arr[high]=temp;
    swapCount++;
    return i+1;
}

void quickSort(Student* arr,int low,int high,int &swapCount){
    if(low<high){
        int pi=partition(arr,low,high,swapCount);
        quickSort(arr,low,pi-1,swapCount);
        quickSort(arr,pi+1,high,swapCount);
    }
}

void display(Student* arr,int n){
    cout<<"\nStudent Details:\n";
    for(int i=0;i<n;i++){
        cout<<i+1<<". "<<arr[i].student_name<<" "<<arr[i].student_roll_no<<" "<<arr[i].total_marks<<endl;
    }
}

int main(){
    srand(0);
    int n;
    cout<<"Enter number of students:";
    cin>>n;
    Student* arr=new Student[n];
    string names[]={"ABC","PQR","XYZ","LMN","DEF"};
    for(int i=0;i<n;i++){
        arr[i].student_name=names[rand()%5];
        arr[i].student_roll_no=rand()%100+1;
        arr[i].total_marks=rand()%101;
    }

    Student* arrBubble=new Student[n];
    Student* arrQuick=new Student[n];
    for(int i=0;i<n;i++){
        arrBubble[i]=arr[i];
        arrQuick[i]=arr[i];
    }

    int choice,swapCount;
    do{
        cout<<"\nMenu:\n1.Bubble Sort\n2.Quick Sort\n3.Display Original Array\n4.Exit\nEnter choice:";
        cin>>choice;
        switch(choice){
            case 1:
                bubbleSort(arrBubble,n,swapCount);
                cout<<"Array after Bubble Sort:\n";
                display(arrBubble,n);
                cout<<"Number of swaps performed: "<<swapCount<<endl;
                break;
            case 2:
                swapCount=0;
                quickSort(arrQuick,0,n-1,swapCount);
                cout<<"Array after Quick Sort:\n";
                display(arrQuick,n);
                cout<<"Number of swaps performed: "<<swapCount<<endl;
                break;
            case 3:
                cout<<"Original Array:\n";
                display(arr,n);
                break;
            case 4:
                cout<<"Exiting...\n";
                break;
            default:
                cout<<"Invalid choice!\n";
        }
    }while(choice!=4);

    delete[] arr;
    delete[] arrBubble;
    delete[] arrQuick;
    return 0;
}
```

## Example Output  
```
Enter number of students:10

Menu:
1.Bubble Sort
2.Quick Sort
3.Display Original Array
4.Exit
Enter choice:3
Original Array:

Student Details:
1. ABC 92 35
2. XYZ 62 64
3. ABC 38 41
4. PQR 95 96
5. PQR 97 52
6. LMN 60 20
7. DEF 15 55
8. LMN 84 72
9. XYZ 42 95
10. XYZ 54 61

Menu:
1.Bubble Sort
2.Quick Sort
3.Display Original Array
4.Exit
Enter choice:1
Array after Bubble Sort:

Student Details:
1. DEF 15 55
2. ABC 38 41
3. XYZ 42 95
4. XYZ 54 61
5. LMN 60 20
6. XYZ 62 64
7. LMN 84 72
8. ABC 92 35
9. PQR 95 96
10. PQR 97 52
Number of swaps performed: 28

Menu:
1.Bubble Sort
2.Quick Sort
3.Display Original Array
4.Exit
Enter choice:2
Array after Quick Sort:

Student Details:
1. DEF 15 55
2. ABC 38 41
3. XYZ 42 95
4. XYZ 54 61
5. LMN 60 20
6. XYZ 62 64
7. LMN 84 72
8. ABC 92 35
9. PQR 95 96
10. PQR 97 52
Number of swaps performed: 22

Menu:
1.Bubble Sort
2.Quick Sort
3.Display Original Array
4.Exit
Enter choice:4
Exiting...
```
