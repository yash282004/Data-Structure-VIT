# Assignment 2: Infix to Postfix Conversion Using Stack

## Author
**Yash Santosh Khandagale**

## AIM
To convert a user-defined infix expression into postfix using stack in C++.

## Problem Statement
Convert infix expression (e.g., a-b*c) into postfix expression using stack.

## C++ Implementation
```cpp
#include <iostream>
#include <cctype>
using namespace std;

char stack[50];
int top = -1;

void push(char c) { stack[++top] = c; }
char pop() { return stack[top--]; }

int priority(char c) {
    if (c == '*' || c == '/') return 2;
    if (c == '+' || c == '-') return 1;
    return 0;
}

int main() {
    string exp;
    cout << "Enter infix expression: ";
    cin >> exp;

    for (char c : exp) {
        if (isalnum(c)) cout << c;
        else {
            while (top != -1 && priority(stack[top]) >= priority(c))
                cout << pop();
            push(c);
        }
    }
    while (top != -1) cout << pop();
    return 0;
}
```

## Sample Output
```
Enter infix expression: a-b*c
abc*-
```
