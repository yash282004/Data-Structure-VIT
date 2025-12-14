# Program by Yash Santosh Khandagale

## Problem Statement / Title
Develop a program to identify and efficiently store a sparse matrix using compact representation and perform basic operations like display and simple transpose

## Description
This program identifies a sparse matrix, converts it into a compact representation to save memory, and performs basic operations such as displaying the compact form and computing its simple transpose.

## Code
```cpp

#include<iostream>
#include<cstdlib>
using namespace std;
typedef struct Sparse{
    int row,col,val;
}SPR;
int main(void){
    int m,n;
    int** matrix=NULL;
    SPR* sparse=NULL;
    int i,j;

    cout<<"Enter the no of rows :"<<endl;
    cin>>m;
    cout<<"Enter the no of cols :"<<endl;
    cin>>n;

    matrix=new int*[m];
    if(matrix==NULL){
        cout<<"Memory Allocation Failed"<<endl;
        exit(-1);
    }
    for(i=0;i<m;i++){
        matrix[i]=new int[n];
        if(matrix[i]==NULL){
            cout<<"Memory Allocation Failed"<<endl;
            exit(-1);
        }
    }

    cout<<"Enter the elements in the Matrix : ";
    int counter=0;
    for(i=0;i<m;i++){
        for(j=0;j<n;j++){
            cin>>matrix[i][j];
            if(matrix[i][j]!=0)
                counter++;
        }
    }

    cout<<"\n*****************************\n";
    cout<<"The Matrix is :"<<endl;
    for(i=0;i<m;i++){
        for(j=0;j<n;j++){
            cout<<matrix[i][j]<<" ";
        }
        cout<<endl;
    }

    sparse=new SPR[counter+1];
    if(sparse==NULL){
        cout<<"Memory Allocation Failed"<<endl;
        exit(-1);
    }
    sparse[0].row=m;
    sparse[0].col=n;
    sparse[0].val=counter;

    int k=1;
    for(i=0;i<m;i++){
        for(j=0;j<n;j++){
            if(matrix[i][j]!=0){
                sparse[k].row=i;
                sparse[k].col=j;
                sparse[k].val=matrix[i][j];
                k++;
            }
        }
    }

    cout<<"\nSparse Matrix : \n";
    for(i=0;i<=counter;i++){
        cout<<sparse[i].row<<"\t"<<sparse[i].col<<"\t"<<sparse[i].val<<endl;
    }

    cout<<"\n\nTranspose of Matrix :"<<endl;
    for(j=0;j<n;j++){
        for(i=0;i<m;i++){
            cout<<matrix[i][j]<<" ";
        }
        cout<<endl;
    }

    for(i=0;i<m;i++){
        delete[] matrix[i];
        matrix[i]=NULL;
    }
    delete[] matrix;
    matrix=NULL;
    delete[] sparse;
    sparse=NULL;

    return 0;
}
```

## Example Output
```

*****************************
The Matrix is :
1 0 0 
2 0 0 
3 0 0 

Sparse Matrix : 
3       3       3
0       0       1
1       0       2
2       0       3


Transpose of Matrix :
1 2 3 
0 0 0 
0 0 0 

```
