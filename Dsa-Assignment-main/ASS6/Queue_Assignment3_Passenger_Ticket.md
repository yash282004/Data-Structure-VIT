# Assignment 3: Passenger Queue for Ticket Counter

## Author
**Sejal Dinesh Revanwar**

## AIM
To manage a queue of passengers waiting for a ticket agent using C++.

## Problem Statement
Maintain a queue of passengers with operations to add, remove, and display remaining passengers.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

#define SIZE 10
int queue[SIZE], front = -1, rear = -1;

void enqueue() {
    int id;
    cout << "Enter passenger ID: ";
    cin >> id;
    if (rear == SIZE - 1) return;
    if (front == -1) front = 0;
    queue[++rear] = id;
}

void dequeue() {
    if (front == -1 || front > rear) return;
    cout << "Passenger served: " << queue[front++] << endl;
}

int main() {
    int ch;
    do {
        cout << "\n1.Add Passenger 2.Serve Passenger 3.Exit\n";
        cin >> ch;
        if (ch == 1) enqueue();
        else if (ch == 2) dequeue();
    } while (ch != 3);

    cout << "Passengers left: " << rear - front + 1 << endl;
    return 0;
}
```

## Sample Output
```
Passenger served: 1
Passengers left: 0
```
