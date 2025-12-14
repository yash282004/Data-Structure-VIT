# Program by Yash Santosh Khandagale

## Problem Statement / Title
Write a program using **Bubble Sort** algorithm, assign the roll nos. to the students of your class as per their previous year's result — i.e., topper will be roll no. 1 — and analyse the sorting algorithm **pass by pass**.

## Description (GeeksforGeeks style — simple & easy)
We have a list of students and their previous-year marks. We want to give roll numbers so that the student with highest marks gets roll number 1, next highest gets roll number 2, and so on.  
We'll use **Bubble Sort** to sort students in **descending order** by marks (so topper comes first). After each pass of the Bubble Sort, we'll print the current order of students to analyse how the algorithm progresses. After sorting, we assign roll numbers (1..n) according to the sorted order.

This helps you understand how Bubble Sort moves the larger elements toward the front (here: higher marks to the front) and lets you study how many swaps happen in each pass and when the array becomes sorted.

## Code
```cpp
#include <iostream>
#include <cstdlib>
using namespace std;

void bubbleSort(int* marks,int n){
    for(int i=0;i<n-1;i++){
        bool swapped=false;
        for(int j=0;j<n-i-1;j++){
            if(marks[j]<marks[j+1]){
                int temp=marks[j];
                marks[j]=marks[j+1];
                marks[j+1]=temp;
                swapped=true;
            }
        }
        cout<<"After pass "<<i+1<<": ";
        for(int k=0;k<n;k++)
            cout<<marks[k]<<" ";
        cout<<endl;
        if(!swapped)
            break;
    }
}

int main(){
    srand(time(0));
    int n;
    cout<<"Enter number of students:";
    cin>>n;
    int* marks=new int[n];
    int* rollNos=new int[n];

    cout<<"Generated marks of students:\n";
    for(int i=0;i<n;i++){
        marks[i]=rand()%101;
        cout<<marks[i]<<" ";
    }
    cout<<endl;

    bubbleSort(marks,n);

    // Assign roll numbers after sorting
    for(int i=0;i<n;i++)
        rollNos[i]=i+1;

    cout<<"\nMarks in descending order and assigned roll numbers:\n";
    for(int i=0;i<n;i++)
        cout<<"Roll No: "<<rollNos[i]<<" Marks: "<<marks[i]<<endl;

    delete[] marks;
    delete[] rollNos;
    return 0;
}
```

## Example Output
```
Enter number of students:5
Generated marks of students:
82 25 81 6 68 
After pass 1: 82 81 25 68 6 
After pass 2: 82 81 68 25 6 
After pass 3: 82 81 68 25 6 

Marks in descending order and assigned roll numbers:
Roll No: 1 Marks: 82
Roll No: 2 Marks: 81
Roll No: 3 Marks: 68
Roll No: 4 Marks: 25
Roll No: 5 Marks: 6
```
