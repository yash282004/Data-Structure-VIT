# Assignment 1: Patient Queue in Medical Clinic

## Author
**Sejal Dinesh Revanwar**

## AIM
To implement a user-defined queue in C++ to manage patients in a medical clinic on a first-come, first-served basis.

## Problem Statement
Design a queue system to manage patients checking into a medical clinic. Patients are assigned to doctors in the order they arrive.

## Algorithm
1. Initialize queue using array.
2. Insert patient ID at rear (enqueue).
3. Remove patient from front (dequeue).
4. Display queue operations using menu.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

#define SIZE 10
int queue[SIZE], front = -1, rear = -1;

void enqueue() {
    int id;
    cout << "Enter patient ID: ";
    cin >> id;

    if (rear == SIZE - 1) {
        cout << "Queue Overflow\n";
        return;
    }
    if (front == -1) front = 0;
    queue[++rear] = id;
}

void dequeue() {
    if (front == -1 || front > rear) {
        cout << "Queue Underflow\n";
        return;
    }
    cout << "Patient " << queue[front++] << " assigned to doctor\n";
}

void display() {
    if (front == -1 || front > rear) {
        cout << "Queue is empty\n";
        return;
    }
    for (int i = front; i <= rear; i++)
        cout << queue[i] << " ";
    cout << endl;
}

int main() {
    int ch;
    do {
        cout << "\n1.Enqueue 2.Dequeue 3.Display 4.Exit\n";
        cin >> ch;
        switch (ch) {
            case 1: enqueue(); break;
            case 2: dequeue(); break;
            case 3: display(); break;
        }
    } while (ch != 4);
    return 0;
}
```

## Sample Output
```
Enter patient ID: 101
Patient 101 assigned to doctor
```
