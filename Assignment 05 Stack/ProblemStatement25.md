# Assignment 5: Postfix Expression Evaluation Using Stack

## Author
**Yash Santosh Khandagale**

## AIM
To evaluate a user-defined postfix expression using stack in C++.

## C++ Implementation
```cpp
#include <iostream>
#include <cctype>
using namespace std;

int stack[50];
int top = -1;

void push(int x) { stack[++top] = x; }
int pop() { return stack[top--]; }

int main() {
    string exp;
    cout << "Enter postfix expression: ";
    cin >> exp;

    for (char c : exp) {
        if (isdigit(c))
            push(c - '0');
        else {
            int b = pop();
            int a = pop();
            switch (c) {
                case '+': push(a + b); break;
                case '-': push(a - b); break;
                case '*': push(a * b); break;
                case '/': push(a / b); break;
            }
        }
    }
    cout << "Result: " << pop();
    return 0;
}
```

## Sample Output
```
Enter postfix expression: 23*5+
Result: 11
```
