# Matrix Multiplication Performance Analysis (Row-Major vs Column-Major)

**Name:** Vaidehi Vinayak Unhale  

**Problem Statement / Title:**  
Implement matrix multiplication and analyse its performance using row-major vs column-major order access patterns to understand how memory layout affects cache performance.

**Description:**  
This program multiplies two matrices using both row-major and column-major access patterns.  
It measures execution time for each method, showing how cache-friendly access (row-major) outperforms cache-unfriendly access (column-major) due to better memory locality.  

---

## Code (C++)

```cpp
#include <bits/stdc++.h>
using namespace std;
using namespace chrono;

// Function to multiply in row-major order
void multiplyRowMajor_vvu(const vector<vector<int>> &A_vvu, const vector<vector<int>> &B_vvu, vector<vector<int>> &C_vvu, int n_vvu) {
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        for (int j_vvu = 0; j_vvu < n_vvu; j_vvu++) {
            int sum_vvu = 0;
            for (int k_vvu = 0; k_vvu < n_vvu; k_vvu++) {
                sum_vvu += A_vvu[i_vvu][k_vvu] * B_vvu[k_vvu][j_vvu];
            }
            C_vvu[i_vvu][j_vvu] = sum_vvu;
        }
    }
}

// Function to multiply in column-major order (simulating poor cache usage)
void multiplyColumnMajor_vvu(const vector<vector<int>> &A_vvu, const vector<vector<int>> &B_vvu, vector<vector<int>> &C_vvu, int n_vvu) {
    for (int j_vvu = 0; j_vvu < n_vvu; j_vvu++) {
        for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
            int sum_vvu = 0;
            for (int k_vvu = 0; k_vvu < n_vvu; k_vvu++) {
                sum_vvu += A_vvu[i_vvu][k_vvu] * B_vvu[k_vvu][j_vvu];
            }
            C_vvu[i_vvu][j_vvu] = sum_vvu;
        }
    }
}

int main() {
    int n_vvu;
    cout << "Enter matrix size (n x n): ";
    cin >> n_vvu;

    vector<vector<int>> A_vvu(n_vvu, vector<int>(n_vvu));
    vector<vector<int>> B_vvu(n_vvu, vector<int>(n_vvu));
    vector<vector<int>> C_vvu(n_vvu, vector<int>(n_vvu));

    // Initialize matrices with random values
    srand(time(0));
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        for (int j_vvu = 0; j_vvu < n_vvu; j_vvu++) {
            A_vvu[i_vvu][j_vvu] = rand() % 100;
            B_vvu[i_vvu][j_vvu] = rand() % 100;
        }
    }

    // Row-major multiplication timing
    auto startRow_vvu = high_resolution_clock::now();
    multiplyRowMajor_vvu(A_vvu, B_vvu, C_vvu, n_vvu);
    auto endRow_vvu = high_resolution_clock::now();
    auto durationRow_vvu = duration_cast<milliseconds>(endRow_vvu - startRow_vvu).count();

    // Column-major multiplication timing
    auto startCol_vvu = high_resolution_clock::now();
    multiplyColumnMajor_vvu(A_vvu, B_vvu, C_vvu, n_vvu);
    auto endCol_vvu = high_resolution_clock::now();
    auto durationCol_vvu = duration_cast<milliseconds>(endCol_vvu - startCol_vvu).count();

    cout << "\nPerformance Analysis for Matrix Multiplication (" << n_vvu << "x" << n_vvu << "):\n";
    cout << "Row-Major Time: " << durationRow_vvu << " ms\n";
    cout << "Column-Major Time: " << durationCol_vvu << " ms\n";

    return 0;
}
```

---

## Sample Output (example run with n=500)

```
Enter matrix size (n x n): 500

Performance Analysis for Matrix Multiplication (500x500):
Row-Major Time: 210 ms
Column-Major Time: 680 ms
```

---


