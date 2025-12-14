# Matrix Multiplication Performance Analysis (Row-Major vs Column-Major)

**Name:** Yash Santosh Khandagale  

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
void multiplyRowMajor(const vector<vector<int>> &A,
                      const vector<vector<int>> &B,
                      vector<vector<int>> &C,
                      int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int sum = 0;
            for (int k = 0; k < n; k++) {
                sum += A[i][k] * B[k][j];
            }
            C[i][j] = sum;
        }
    }
}

// Function to multiply in column-major order (poor cache usage)
void multiplyColumnMajor(const vector<vector<int>> &A,
                         const vector<vector<int>> &B,
                         vector<vector<int>> &C,
                         int n) {
    for (int j = 0; j < n; j++) {
        for (int i = 0; i < n; i++) {
            int sum = 0;
            for (int k = 0; k < n; k++) {
                sum += A[i][k] * B[k][j];
            }
            C[i][j] = sum;
        }
    }
}

int main() {
    int n;
    cout << "Enter matrix size (n x n): ";
    cin >> n;

    vector<vector<int>> A(n, vector<int>(n));
    vector<vector<int>> B(n, vector<int>(n));
    vector<vector<int>> C(n, vector<int>(n));

    // Initialize matrices with random values
    srand(time(0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            A[i][j] = rand() % 100;
            B[i][j] = rand() % 100;
        }
    }

    // Row-major multiplication timing
    auto startRow = high_resolution_clock::now();
    multiplyRowMajor(A, B, C, n);
    auto endRow = high_resolution_clock::now();
    auto durationRow = duration_cast<milliseconds>(endRow - startRow).count();

    // Column-major multiplication timing
    auto startCol = high_resolution_clock::now();
    multiplyColumnMajor(A, B, C, n);
    auto endCol = high_resolution_clock::now();
    auto durationCol = duration_cast<milliseconds>(endCol - startCol).count();

    cout << "\nPerformance Analysis for Matrix Multiplication (" << n << "x" << n << "):\n";
    cout << "Row-Major Time: " << durationRow << " ms\n";
    cout << "Column-Major Time: " << durationCol << " ms\n";

    return 0;
}

```

---

## Sample Output (example run with n=500)

```
Enter matrix size (n x n): 500

Performance Analysis for Matrix Multiplication (500x500):
Row-Major Time: 430 ms
Column-Major Time: 690 ms

```

---


