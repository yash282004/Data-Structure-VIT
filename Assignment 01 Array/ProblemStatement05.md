# Program by Yash Santosh Khandagale

## Problem Statement / Title  
Develop a program to compute the **fast transpose** of a sparse matrix using its compact (triplet) representation efficiently.

## Description  
A sparse matrix has mostly zero elements. Instead of storing all elements (including zeros), we use **compact representation** (triplets) which store only non-zero entries as `(row, column, value)`.

Fast transpose means swapping rows and columns of these non-zero entries **efficiently**. Instead of scanning the entire matrix again and again, we:
1. Count how many non-zero elements are in each column of the original compact matrix.
2. Compute the starting positions of those in the transpose.
3. Place them directly at the correct position in one pass.

This avoids repeated scanning and is much faster for large sparse matrices.

## Code  
```cpp
#include <iostream>
using namespace std;

void createCompact(int** matrix,int** compact,int rows,int cols){
    int k=1;
    compact[0][0]=rows;
    compact[0][1]=cols;
    for(int i=0;i<rows;i++){
        for(int j=0;j<cols;j++){
            if(matrix[i][j]!=0){
                compact[k][0]=i;
                compact[k][1]=j;
                compact[k][2]=matrix[i][j];
                k++;
            }
        }
    }
    compact[0][2]=k-1;
}

void displayCompact(int** compact){
    int nz=compact[0][2];
    cout<<"Row\tCol\tValue\n";
    for(int i=0;i<=nz;i++){
        cout<<compact[i][0]<<"\t"<<compact[i][1]<<"\t"<<compact[i][2]<<"\n";
    }
}

void fastTranspose(int** compact,int** transpose){
    int rows=compact[0][0];
    int cols=compact[0][1];
    int nz=compact[0][2];
    transpose[0][0]=cols;
    transpose[0][1]=rows;
    transpose[0][2]=nz;

    int* count=new int[cols]{0};
    int* index=new int[cols]{0};

    for(int i=1;i<=nz;i++){
        count[compact[i][1]]++;
    }

    index[0]=1;
    for(int j=1;j<cols;j++){
        index[j]=index[j-1]+count[j-1];
    }

    for(int i=1;i<=nz;i++){
        int c=compact[i][1];
        int pos=index[c];
        transpose[pos][0]=compact[i][1];
        transpose[pos][1]=compact[i][0];
        transpose[pos][2]=compact[i][2];
        index[c]++;
    }

    delete[] count;
    delete[] index;
}

int main(){
    int rows,cols;
    cout<<"Enter rows and columns of the matrix:";
    cin>>rows>>cols;

    int** matrix=new int*[rows];
    for(int i=0;i<rows;i++)
        matrix[i]=new int[cols];

    int** compact=new int*[rows*cols+1];
    for(int i=0;i<rows*cols+1;i++)
        compact[i]=new int[3];

    int** transpose=new int*[rows*cols+1];
    for(int i=0;i<rows*cols+1;i++)
        transpose[i]=new int[3];

    cout<<"Enter elements of the matrix:\n";
    for(int i=0;i<rows;i++){
        for(int j=0;j<cols;j++){
            cin>>matrix[i][j];
        }
    }

    createCompact(matrix,compact,rows,cols);

    cout<<"\nCompact Representation:\n";
    displayCompact(compact);

    fastTranspose(compact,transpose);

    cout<<"\nFast Transpose of Compact Representation:\n";
    displayCompact(transpose);

    for(int i=0;i<rows;i++)
        delete[] matrix[i];
    delete[] matrix;

    for(int i=0;i<rows*cols+1;i++){
        delete[] compact[i];
        delete[] transpose[i];
    }
    delete[] compact;
    delete[] transpose;

    return 0;
}
```

## Example Output  
```
Enter rows and columns of the matrix:4
4
Enter elements of the matrix:
0
0
0
1
0
0
0
2
0
0
0
3
0
0
0
4

Compact Representation:
Row     Col     Value
4       4       4
0       3       1
1       3       2
2       3       3
3       3       4

Fast Transpose of Compact Representation:
Row     Col     Value
4       4       4
3       0       1
3       1       2
3       2       3
3       3       4
yashkhandagale@Yashs-MacBook-Air Assignments % 
```
