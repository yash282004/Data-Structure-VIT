# Program by Yash Santosh Khandagale

## Problem Statement / Title  
In Computer Engineering Department of VIT, there are S.Y., T.Y., and B.Tech students. Assume all these students are on the ground for a function. We need to identify a student of S.Y. Div. (X) whose name is "XYZ" and roll no. is "17". Apply an appropriate searching method to identify the required student.

## Description  
We are given a list of students from different classes. We want to search for a **specific student** (S.Y. Div. X, name = XYZ, roll no. = 17).  
Since the data might not be sorted, we use **Linear Search** to go through each student one by one until we find a match.

## Code  
```cpp
#include <iostream>
#include <stdlib.h>
using namespace std;

struct Student{
    string year;
    string division;
    string name;
    int rollNo;
};

int main(){
    int n;
    cout<<"Enter number of students:";
    cin>>n;
    Student* students=new Student[n];
    string years[]={"SY","TY","BTECH"};
    string divisions[]={"X","Y"};
    string names[]={"ABC","PQR","XYZ"};
    for(int i=0;i<n;i++){
        students[i].year=years[rand()%3];
        students[i].division=divisions[rand()%3];
        students[i].name=names[rand()%5];
        students[i].rollNo=rand()%20+1;
    }
    cout<<"\nGenerated Student Data:\n";
    for(int i=0;i<n;i++){
        cout<<i+1<<"."<<students[i].year<<" "<<students[i].division<<" "<<students[i].name<<" "<<students[i].rollNo<<endl;
    }
    string searchYear="SY";
    string searchDiv="X";
    string searchName="XYZ";
    int searchRoll=17;
    bool found=false;
    for(int i=0;i<n;i++){
        if(students[i].year==searchYear&&students[i].division==searchDiv&&students[i].name==searchName&&students[i].rollNo==searchRoll){
            cout<<"\nStudent Found!\n";
            cout<<"Year:"<<students[i].year<<endl;
            cout<<"Division:"<<students[i].division<<endl;
            cout<<"Name:"<<students[i].name<<endl;
            cout<<"Roll No:"<<students[i].rollNo<<endl;
            found=true;
            break;
        }
    }
    if(!found)
        cout<<"\nStudent Not Found!\n";
    delete[] students;
    return 0;
}

```

## Example Output  
```
Student Found!
Year:SY
Division:X
Name:XYZ
Roll No:17
```
