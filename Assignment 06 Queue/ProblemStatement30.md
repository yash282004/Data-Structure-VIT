# Assignment 5: Call Center Queue Using Linked List

## Author
**Yash Santosh Khandagale**

## AIM
To implement a user-defined call center queue using linked list in C++.

## Problem Statement
Handle customer calls in a call center using FCFS principle.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

struct Node {
    int callID;
    Node* next;
};

Node *front = NULL, *rear = NULL;

void enqueue() {
    int id;
    cout << "Enter call ID: ";
    cin >> id;
    Node* temp = new Node();
    temp->callID = id;
    temp->next = NULL;

    if (!rear)
        front = rear = temp;
    else {
        rear->next = temp;
        rear = temp;
    }
}

void dequeue() {
    if (!front) {
        cout << "No calls waiting\n";
        return;
    }
    cout << "Call handled: " << front->callID << endl;
    Node* temp = front;
    front = front->next;
    delete temp;
}

int main() {
    int ch;
    do {
        cout << "\n1.Receive Call 2.Handle Call 3.Exit\n";
        cin >> ch;
        if (ch == 1) enqueue();
        else if (ch == 2) dequeue();
    } while (ch != 3);
    return 0;
}
```

## Sample Output
```
Enter call ID: 1
Call handled: 1
```
