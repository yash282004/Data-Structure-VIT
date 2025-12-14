# Assignment 3: Multiple Stacks Using Array

## Author
**Yash Santosh Khandagale**

## AIM
To implement multiple stacks using array in C++ with user-defined operations.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

#define SIZE 5
int stack[3][SIZE];
int top[3] = {-1, -1, -1};

void push(int s) {
    int val;
    cout << "Enter value: ";
    cin >> val;
    if (top[s] == SIZE - 1) {
        cout << "Stack Overflow\n";
        return;
    }
    stack[s][++top[s]] = val;
}

void pop(int s) {
    if (top[s] == -1) {
        cout << "Stack Underflow\n";
        return;
    }
    cout << "Popped: " << stack[s][top[s]--] << endl;
}

int main() {
    int ch, s;
    do {
        cout << "\n1.Push 2.Pop 3.Exit\n";
        cin >> ch;
        if (ch == 1 || ch == 2) {
            cout << "Enter stack number (0-2): ";
            cin >> s;
            if (ch == 1) push(s);
            else pop(s);
        }
    } while (ch != 3);
    return 0;
}
```

## Sample Output
```
Popped: 20
```
