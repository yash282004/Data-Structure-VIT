# Assignment 4: Balanced Parentheses Using Stack

## Author
**Sejal Dinesh Revanwar**

## AIM
To check whether a user-defined expression has balanced parentheses using stack.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

char stack[50];
int top = -1;

void push(char c) { stack[++top] = c; }
void pop() { top--; }

int main() {
    string exp;
    cout << "Enter expression: ";
    cin >> exp;

    for (char c : exp) {
        if (c == '(' || c == '{' || c == '[')
            push(c);
        else {
            if (top == -1) {
                cout << "Not Balanced";
                return 0;
            }
            pop();
        }
    }
    if (top == -1) cout << "Balanced";
    else cout << "Not Balanced";
    return 0;
}
```

## Sample Output
```
Enter expression: {[()]}
Balanced
```
