# Program by Vaidehi Vinayak Unhale

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

void createCompact_vvu(int matrix_vvu[10][10], int compact_vvu[30][3], int rows_vvu, int cols_vvu) {
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
    compact_vvu[0][2] = k_vvu - 1;  // number of non-zero elements
}

void displayCompact_vvu(int compact_vvu[30][3]) {
    int nz_vvu = compact_vvu[0][2];
    cout << "Row\tCol\tValue\n";
    for (int i_vvu = 0; i_vvu <= nz_vvu; i_vvu++) {
        cout << compact_vvu[i_vvu][0] << "\t"
             << compact_vvu[i_vvu][1] << "\t"
             << compact_vvu[i_vvu][2] << "\n";
    }
}

void fastTranspose_vvu(int compact_vvu[30][3], int transpose_vvu[30][3]) {
    int rows_vvu = compact_vvu[0][0];
    int cols_vvu = compact_vvu[0][1];
    int nz_vvu   = compact_vvu[0][2];

    transpose_vvu[0][0] = cols_vvu;
    transpose_vvu[0][1] = rows_vvu;
    transpose_vvu[0][2] = nz_vvu;

    int count_vvu[50] = {0};
    int index_vvu[50] = {0};

    // Count how many non-zero entries are in each column of original
    for (int i_vvu = 1; i_vvu <= nz_vvu; i_vvu++) {
        count_vvu[ compact_vvu[i_vvu][1] ]++;
    }

    // Compute starting index in transpose for each column
    index_vvu[0] = 1;
    for (int j_vvu = 1; j_vvu < cols_vvu; j_vvu++) {
        index_vvu[j_vvu] = index_vvu[j_vvu - 1] + count_vvu[j_vvu - 1];
    }

    // Place entries directly in correct position in transpose
    for (int i_vvu = 1; i_vvu <= nz_vvu; i_vvu++) {
        int c_vvu = compact_vvu[i_vvu][1];
        int pos_vvu = index_vvu[c_vvu];

        transpose_vvu[pos_vvu][0] = compact_vvu[i_vvu][1];
        transpose_vvu[pos_vvu][1] = compact_vvu[i_vvu][0];
        transpose_vvu[pos_vvu][2] = compact_vvu[i_vvu][2];

        index_vvu[c_vvu]++;
    }
}

int main() {
    int rows_vvu, cols_vvu;
    int matrix_vvu[10][10], compact_vvu[30][3], transpose_vvu[30][3];

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

    fastTranspose_vvu(compact_vvu, transpose_vvu);

    cout << "\nFast Transpose of Compact Representation:\n";
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

Fast Transpose of Compact Representation:
Row    Col    Value
3      3      2
2      0      5
1      2      7
```
