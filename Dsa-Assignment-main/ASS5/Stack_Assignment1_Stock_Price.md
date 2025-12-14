# Assignment 1: Stock Price Tracker Using Stack (Linked List)

## Author
**Sejal Dinesh Revanwar**

## AIM
To implement a user-defined stack using linked list in C++ to track daily stock prices.

## Problem Statement
Build a stock price tracker that stores daily stock prices and allows the user to:
- Record a new price
- Remove the latest price
- View the latest price
- Check if stack is empty

## Algorithm
1. Create a node structure with data and next pointer.
2. Push operation inserts node at top.
3. Pop operation removes node from top.
4. Peek displays top element.
5. Menu-driven execution.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

struct Node {
    int price;
    Node* next;
};

Node* top = NULL;

void push() {
    int price;
    cout << "Enter stock price: ";
    cin >> price;
    Node* temp = new Node();
    temp->price = price;
    temp->next = top;
    top = temp;
}

void pop() {
    if (!top) {
        cout << "Stack is empty\n";
        return;
    }
    cout << "Removed price: " << top->price << endl;
    Node* temp = top;
    top = top->next;
    delete temp;
}

void peek() {
    if (!top) cout << "No prices recorded\n";
    else cout << "Latest price: " << top->price << endl;
}

void isEmpty() {
    if (!top) cout << "Stack is empty\n";
    else cout << "Stack is not empty\n";
}

int main() {
    int ch;
    do {
        cout << "\n1.Push 2.Pop 3.Peek 4.IsEmpty 5.Exit\n";
        cin >> ch;
        switch (ch) {
            case 1: push(); break;
            case 2: pop(); break;
            case 3: peek(); break;
            case 4: isEmpty(); break;
        }
    } while (ch != 5);
    return 0;
}
```

## Sample Output
```
Enter stock price: 450
Latest price: 450
```
