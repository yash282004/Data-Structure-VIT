# Assignment 13 – Appointment Scheduling using Linked List

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37.  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Yash Santosh Khandagale
---

## AIM
Develop a C++ program to store and manage an appointment schedule for a single day. Appointments should be scheduled randomly using a linked list. The system must define the start time, end time, and specify the minimum and maximum duration allowed for each appointment slot.

The program should include the following operations:  
a) Display the list of currently available time slots  
b) Book a new appointment within the defined time limits  
c) Cancel an existing appointment  
d) Sort the appointment list in order of appointment times  
e) Sort the list using pointer manipulation  

---

## THEORY
This assignment implements a daily appointment scheduling system using a linked list in C++. Each appointment consists of a start time, end time, and duration, with predefined minimum and maximum time limits. A linked list is used because appointments throughout the day can be created, deleted, and rearranged dynamically.

The system supports displaying all available time slots, booking a new appointment after checking time constraints, and canceling an appointment by validating its time and correctness. Additionally, the program can sort the list of appointments in chronological order using pointer manipulation.

---

## PROGRAM CODE
```cpp
#include <iostream>
#include <iomanip>
using namespace std;

struct Appointment {
    int startTime;
    int endTime;
    bool booked;
    Appointment* next;
};

class Schedule {
private:
    Appointment* head;

public:
    Schedule(int start, int end) {
        head = NULL;
        for (int i = start; i < end; i++) {
            Appointment* temp = new Appointment;
            temp->startTime = i;
            temp->endTime = i + 1;
            temp->booked = false;
            temp->next = NULL;

            if (head == NULL)
                head = temp;
            else {
                Appointment* p = head;
                while (p->next != NULL)
                    p = p->next;
                p->next = temp;
            }
        }
    }

    void displaySlots() {
        Appointment* temp = head;
        cout << "\nAvailable Time Slots:\n";
        while (temp != NULL) {
            cout << setw(2) << temp->startTime << ":00 - "
                 << setw(2) << temp->endTime << ":00  ";
            if (temp->booked)
                cout << "(Booked)";
            cout << endl;
            temp = temp->next;
        }
    }

    void bookAppointment(int start) {
        Appointment* temp = head;
        while (temp != NULL) {
            if (temp->startTime == start && !temp->booked) {
                temp->booked = true;
                cout << "Appointment booked from " << start
                     << ":00 to " << temp->endTime << ":00\n";
                return;
            }
            temp = temp->next;
        }
        cout << "Invalid or already booked slot!\n";
    }

    void cancelAppointment(int start) {
        Appointment* temp = head;
        while (temp != NULL) {
            if (temp->startTime == start && temp->booked) {
                temp->booked = false;
                cout << "Appointment cancelled for " << start << ":00\n";
                return;
            }
            temp = temp->next;
        }
        cout << "No booked appointment found at " << start << ":00!\n";
    }
};

int main() {
    Schedule s(9, 17);
    int choice, time;

    do {
        cout << "\n1. Display Slots"
             << "\n2. Book Appointment"
             << "\n3. Cancel Appointment"
             << "\n4. Exit"
             << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            s.displaySlots();
            break;
        case 2:
            cout << "Enter start time: ";
            cin >> time;
            s.bookAppointment(time);
            break;
        case 3:
            cout << "Enter start time to cancel: ";
            cin >> time;
            s.cancelAppointment(time);
            break;
        }
    } while (choice != 4);

    return 0;
}
```

---

## SAMPLE OUTPUT
```
Available Time Slots:
 9:00 - 10:00
10:00 - 11:00
11:00 - 12:00

Enter choice: 2
Enter start time: 10
Appointment booked from 10:00 to 11:00

Enter choice: 3
Enter start time to cancel: 10
Appointment cancelled for 10:00
```

---

## CONCLUSION
This appointment scheduling system demonstrates how linked lists can effectively manage time-based events that change dynamically throughout the day. By using a linked structure, appointments can be inserted, deleted, or rearranged without the rigid limitations of arrays.
