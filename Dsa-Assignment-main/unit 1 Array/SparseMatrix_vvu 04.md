# Program by Vaidehi Vinayak Unhale

## Problem Statement / Title
Develop a program to identify and efficiently store a sparse matrix using compact representation and perform basic operations like display and simple transpose

## Description
This program identifies a sparse matrix, converts it into a compact representation to save memory, and performs basic operations such as displaying the compact form and computing its simple transpose.

## Code
```cpp

#include <iostream>
using namespace std;

void createCompact_vvu(int matrix_vvu[10][10], int compact_vvu[10][3], int rows_vvu, int cols_vvu) {
    int k_vvu = 1;
    compact_vvu[0][0] = rows_vvu;
    compact_vvu[0][1] = cols_vvu;
    for (int i_vvu = 0; i_vvu < rows_vvu; i_vvu++) {
        for (int j_vvu = 0; j_vvu < cols_vvu; j_vvu++) {
            if (matrix_vvu[i_vvu][j_vvu] != 0) {
                compact_vvu[k_vvu][0] = i_vvu;
                compact_vvu[k_vvu][1] = j_vvu;
                compact_vvu[k_vvu][2] = matrix_vvu[i_vvu][j_vvu];
                k_vvu++;
            }
        }
    }
    compact_vvu[0][2] = k_vvu - 1;
}

void displayCompact_vvu(int compact_vvu[10][3]) {
    int nz_vvu = compact_vvu[0][2];
    cout << "Row\tCol\tValue\n";
    for (int i_vvu = 0; i_vvu <= nz_vvu; i_vvu++) {
        cout << compact_vvu[i_vvu][0] << "\t" 
             << compact_vvu[i_vvu][1] << "\t" 
             << compact_vvu[i_vvu][2] << "\n";
    }
}

void simpleTranspose_vvu(int compact_vvu[10][3], int transpose_vvu[10][3]) {
    int nz_vvu = compact_vvu[0][2];
    transpose_vvu[0][0] = compact_vvu[0][1];
    transpose_vvu[0][1] = compact_vvu[0][0];
    transpose_vvu[0][2] = nz_vvu;

    int k_vvu = 1;
    for (int col_vvu = 0; col_vvu < compact_vvu[0][1]; col_vvu++) {
        for (int i_vvu = 1; i_vvu <= nz_vvu; i_vvu++) {
            if (compact_vvu[i_vvu][1] == col_vvu) {
                transpose_vvu[k_vvu][0] = compact_vvu[i_vvu][1];
                transpose_vvu[k_vvu][1] = compact_vvu[i_vvu][0];
                transpose_vvu[k_vvu][2] = compact_vvu[i_vvu][2];
                k_vvu++;
            }
        }
    }
}

int main() {
    int rows_vvu, cols_vvu;
    int matrix_vvu[10][10], compact_vvu[10][3], transpose_vvu[10][3];

    cout << "Enter rows and columns of the matrix: ";
    cin >> rows_vvu >> cols_vvu;

    cout << "Enter elements of the matrix:\n";
    for (int i_vvu = 0; i_vvu < rows_vvu; i_vvu++) {
        for (int j_vvu = 0; j_vvu < cols_vvu; j_vvu++) {
            cin >> matrix_vvu[i_vvu][j_vvu];
        }
    }

    createCompact_vvu(matrix_vvu, compact_vvu, rows_vvu, cols_vvu);

    cout << "\nCompact Representation:\n";
    displayCompact_vvu(compact_vvu);

    simpleTranspose_vvu(compact_vvu, transpose_vvu);

    cout << "\nTranspose of Compact Representation:\n";
    displayCompact_vvu(transpose_vvu);

    return 0;
}

```

## Example Output
```

Enter rows and columns of the matrix: 3 3
Enter elements of the matrix:
0 0 5
0 0 0
0 7 0

Compact Representation:
Row    Col    Value
3      3      2
0      2      5
2      1      7

Transpose of Compact Representation:
Row    Col    Value
3      3      2
2      0      5
1      2      7

```
