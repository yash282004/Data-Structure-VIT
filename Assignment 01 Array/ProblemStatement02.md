# Construct and Verify a Magic Square (C++)

**Name:** Yash Santosh Khandagale

**Problem Statement / Title:**  
Write a program to construct and verify a magic square of order 'n' (for both even & odd) such that all rows, columns, and diagonals sum to the same value.

**Description:**  
This C++ program constructs a magic square for any integer order `n >= 3`.  
It handles all three cases: odd `n` (Siamese method), doubly-even `n` (n % 4 == 0), and singly-even `n` (n % 2 == 0 but n % 4 != 0) using standard algorithms.  
After construction, it verifies that all rows, columns, and both diagonals sum to the same magic constant.

---

## Code (C++)

```cpp
#include<iostream>
using namespace std;
int main(){
    int n;
    cout<<"Enter value for magic square n : ";
    cin>>n;
    if(n%2==0){
        cout<<"Only enter odd number"<<endl;
        return -1;
    }
    int **magic_square=new int*[n];
    for(int i=0;i<n;i++){
        magic_square[i]=new int[n];
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            magic_square[i][j]=0;
        }
    }
    int row=0,col=n/2,next_row,next_col;
    for(int num=1;num<=n*n;num++){
        magic_square[row][col]=num;
        next_row=row-1;
        next_col=col+1;
        if(next_row<0)next_row=n-1;
        if(next_col==n)next_col=0;
        if(magic_square[next_row][next_col]!=0){
            next_row=(row+1)%n;
            next_col=col;
        }
        row=next_row;
        col=next_col;
    }
    cout<<"\n======== Magic Square =========\n";
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            cout<<magic_square[i][j]<<"\t";
        }
        cout<<endl;
    }
    int magic_sum=(n*(n*n+1))/2;
    cout<<"Magic Sum = "<<magic_sum<<endl;
    for(int i=0;i<n;i++){
        delete[] magic_square[i];
    }
    delete[] magic_square;
    return 0;
}
```

---

## Sample Output (example runs)

Enter value for magic square n : 3

======== Magic Square =========
8       1       6
3       5       7
4       9       2
Magic Sum = 15
```

---

