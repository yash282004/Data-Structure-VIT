# Assignment 2: Pizza Parlour Using Circular Queue

## Author
**Yash Santosh Khandagale**

## AIM
To simulate a pizza parlour accepting orders using circular queue in C++.

## Problem Statement
Pizza parlour accepts maximum n orders. Orders are served on FCFS basis using circular queue.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

#define SIZE 5
int cq[SIZE], front = -1, rear = -1;

void enqueue() {
    int order;
    cout << "Enter order number: ";
    cin >> order;

    if ((rear + 1) % SIZE == front) {
        cout << "Queue Full\n";
        return;
    }
    if (front == -1) front = 0;
    rear = (rear + 1) % SIZE;
    cq[rear] = order;
}

void dequeue() {
    if (front == -1) {
        cout << "Queue Empty\n";
        return;
    }
    cout << "Order served: " << cq[front] << endl;
    if (front == rear)
        front = rear = -1;
    else
        front = (front + 1) % SIZE;
}

int main() {
    int ch;
    do {
        cout << "\n1.Place Order 2.Serve Order 3.Exit\n";
        cin >> ch;
        if (ch == 1) enqueue();
        else if (ch == 2) dequeue();
    } while (ch != 3);
    return 0;
}
```

## Sample Output
```
Enter order number: 1
Order served: 1
```
