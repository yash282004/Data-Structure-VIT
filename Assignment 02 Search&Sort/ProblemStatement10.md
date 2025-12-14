# Program by Yash Santosh Khandagale  

## Problem Statement / Title  
Write a program to arrange the list of employees as per the **average of their height and weight** by using **Merge Sort** and **Selection Sort**. Analyse their time complexities and conclude which algorithm will take less time to sort the list.

## Description (Easy Style)  
We have a list of employees, each with height and weight. We calculate the **average** of (height + weight) / 2 for each employee.  
We will:
- Sort employees based on this average using **Merge Sort**  
- Sort employees based on this average using **Selection Sort**  
- Compare time complexities:  
  - Merge Sort → **O(n log n)**  
  - Selection Sort → **O(n²)**  
Then conclude which is faster.

## Code  

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

struct Employee{
    string name;
    float height;
    float weight;
    float avg;
};

void calculateAverage(Employee* emp,int n){
    for(int i=0;i<n;i++)
        emp[i].avg=(emp[i].height+emp[i].weight)/2;
}

void selectionSort(Employee* emp,int n){
    for(int i=0;i<n-1;i++){
        int minIdx=i;
        for(int j=i+1;j<n;j++){
            if(emp[j].avg<emp[minIdx].avg)
                minIdx=j;
        }
        if(minIdx!=i){
            Employee temp=emp[i];
            emp[i]=emp[minIdx];
            emp[minIdx]=temp;
        }
    }
}

void merge(Employee* emp,int l,int m,int r){
    int n1=m-l+1;
    int n2=r-m;
    Employee* L=new Employee[n1];
    Employee* R=new Employee[n2];
    for(int i=0;i<n1;i++) L[i]=emp[l+i];
    for(int i=0;i<n2;i++) R[i]=emp[m+1+i];
    int i=0,j=0,k=l;
    while(i<n1 && j<n2){
        if(L[i].avg<=R[j].avg) emp[k++]=L[i++];
        else emp[k++]=R[j++];
    }
    while(i<n1) emp[k++]=L[i++];
    while(j<n2) emp[k++]=R[j++];
    delete[] L;
    delete[] R;
}

void mergeSort(Employee* emp,int l,int r){
    if(l<r){
        int m=l+(r-l)/2;
        mergeSort(emp,l,m);
        mergeSort(emp,m+1,r);
        merge(emp,l,m,r);
    }
}

int main(){
    srand(time(0));
    int n;
    cout<<"Enter number of employees:";
    cin>>n;
    Employee* emp1=new Employee[n];
    Employee* emp2=new Employee[n];

    string names[]={"John","Alice","Bob","Eve","Mike","Sara","Tom","Anna","Rick","Lily"};

    for(int i=0;i<n;i++){
        emp1[i].name=names[rand()%10];
        emp1[i].height=150+rand()%51;
        emp1[i].weight=50+rand()%51;
        emp2[i]=emp1[i];
    }

    calculateAverage(emp1,n);
    calculateAverage(emp2,n);

    clock_t start=clock();
    selectionSort(emp1,n);
    clock_t end=clock();
    double timeSelection=(double)(end-start)/CLOCKS_PER_SEC;

    start=clock();
    mergeSort(emp2,0,n-1);
    end=clock();
    double timeMerge=(double)(end-start)/CLOCKS_PER_SEC;

    cout<<"Time taken by Selection Sort: "<<timeSelection<<" seconds"<<endl;
    cout<<"Time taken by Merge Sort: "<<timeMerge<<" seconds"<<endl;

    delete[] emp1;
    delete[] emp2;
    return 0;
}
```

## Example Output  
```
Enter number of employees:100000
Time taken by Selection Sort: 4.02707 seconds
Time taken by Merge Sort: 0.03298 seconds
```
