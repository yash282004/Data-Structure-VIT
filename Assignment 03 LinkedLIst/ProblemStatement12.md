# Assignment 12 – Galaxy Multiplex Ticket Reservation using Doubly Circular Linked List

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37.  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Yash Santosh Khandagale


---

## AIM
The ticket reservation system for Galaxy Multiplex is to be implemented using a C++ program.  
The multiplex has **8 rows with 8 seats in each row**.  
A **doubly circular linked list** will be used to track the availability of seats in each row.

The system should support:
a) Display the current list of available seats  
b) Book one or more seats  
c) Cancel an existing booking  

---

## THEORY
In this assignment, a ticket reservation system for Galaxy Multiplex is implemented using a **doubly circular linked list**, where each node contains links to both the next and previous nodes, and the last node links back to the first node.

Each row of seats is represented by its own doubly circular linked list, and an array stores the head pointer of each row. This allows efficient traversal, booking, and cancellation without shifting elements.

---

## PROGRAM CODE
```cpp
#include<iostream>
#include<stdlib.h>
using namespace std;

class Node {
public:
    int seat_no;
    int status; // 0 = Available, 1 = Booked
    Node* next;
    Node* prev;
};

class TicketReservation {
    Node* head[8];

public:
    TicketReservation() {
        for(int i = 0; i < 8; i++) {
            head[i] = NULL;
            createRow(i);
        }
    }

    void createRow(int row) {
        Node* temp, *p;
        for(int i = 1; i <= 8; i++) {
            temp = new Node();
            temp->seat_no = i;
            temp->status = rand() % 2;
            temp->next = temp->prev = NULL;

            if(head[row] == NULL) {
                head[row] = temp;
                head[row]->next = head[row];
                head[row]->prev = head[row];
            } else {
                p = head[row]->prev;
                p->next = temp;
                temp->prev = p;
                temp->next = head[row];
                head[row]->prev = temp;
            }
        }
    }

    void displaySeats() {
        for(int i = 0; i < 8; i++) {
            cout << "Row " << i + 1 << ": ";
            Node* temp = head[i];
            do {
                if(temp->status == 0)
                    cout << "[A] ";
                else
                    cout << "[B] ";
                temp = temp->next;
            } while(temp != head[i]);
            cout << endl;
        }
    }

    void bookSeat(int row, int seat) {
        Node* temp = head[row - 1];
        for(int i = 1; i < seat; i++)
            temp = temp->next;

        if(temp->status == 1)
            cout << "Seat already booked!\n";
        else {
            temp->status = 1;
            cout << "Seat booked successfully!\n";
        }
    }

    void cancelSeat(int row, int seat) {
        Node* temp = head[row - 1];
        for(int i = 1; i < seat; i++)
            temp = temp->next;

        if(temp->status == 0)
            cout << "Seat already available!\n";
        else {
            temp->status = 0;
            cout << "Booking cancelled successfully!\n";
        }
    }
};

int main() {
    TicketReservation t;
    int choice, row, seat;

    while(true) {
        cout << "\n1. Display Available Seats";
        cout << "\n2. Book Seat";
        cout << "\n3. Cancel Booking";
        cout << "\n4. Exit";
        cout << "\nEnter your choice: ";
        cin >> choice;

        switch(choice) {
            case 1:
                t.displaySeats();
                break;
            case 2:
                cout << "Enter Row (1-8): ";
                cin >> row;
                cout << "Enter Seat (1-8): ";
                cin >> seat;
                t.bookSeat(row, seat);
                break;
            case 3:
                cout << "Enter Row (1-8): ";
                cin >> row;
                cout << "Enter Seat (1-8): ";
                cin >> seat;
                t.cancelSeat(row, seat);
                break;
            case 4:
                exit(0);
            default:
                cout << "Invalid choice!\n";
        }
    }
    return 0;
}
```

---

## SAMPLE OUTPUT
```
1. Display Available Seats
2. Book Seat
3. Cancel Booking
4. Exit
Enter your choice: 1

Row 1: [B] [A] [A] [B] [A] [A] [B] [A]
Row 2: [A] [B] [A] [A] [A] [B] [A] [B]

Enter your choice: 2
Enter Row (1-8): 1
Enter Seat (1-8): 2
Seat booked successfully!

Enter your choice: 3
Enter Row (1-8): 1
Enter Seat (1-8): 2
Booking cancelled successfully!
```

---

## CONCLUSION
This assignment demonstrates how a doubly circular linked list can efficiently manage seat reservations, bookings, and cancellations in real-world systems such as movie multiplexes.
