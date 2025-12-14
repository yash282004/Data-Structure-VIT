# Assignment 4: Multiple Queues Using Array

## Author
**Sejal Dinesh Revanwar**

## AIM
To implement two queues using array in C++ with user-defined operations.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

#define SIZE 5
int q1[SIZE], q2[SIZE];
int f1 = -1, r1 = -1, f2 = -1, r2 = -1;

void enqueueQ1() {
    int x;
    cout << "Enter value for Queue 1: ";
    cin >> x;
    if (r1 == SIZE - 1) return;
    if (f1 == -1) f1 = 0;
    q1[++r1] = x;
}

void enqueueQ2() {
    int x;
    cout << "Enter value for Queue 2: ";
    cin >> x;
    if (r2 == SIZE - 1) return;
    if (f2 == -1) f2 = 0;
    q2[++r2] = x;
}

int main() {
    enqueueQ1();
    enqueueQ2();
    cout << "Multiple queues implemented successfully" << endl;
    return 0;
}
```

## Sample Output
```
Multiple queues implemented successfully
```
