# Assignment 14 – Cricket and Football Set Operations using Linked List

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37.  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Vaidehi Vinayak Unhale  
**Roll Number:** 12  
**Division:** SEDA [A]  
**Subject:** Data Structure  
**Assignment No:** 03  
**Problem Statement:** 04  

---

## AIM
In the Second Year Computer Engineering class, there are two groups of students based on their favorite sports:
- Set A includes students who like **Cricket**
- Set B includes students who like **Football**

Write a C++ program to represent these two sets using linked lists and perform the following operations:  
a) Find students who like both Cricket and Football  
b) Find students who like either Cricket or Football but not both  
c) Display the number of students who like neither Cricket nor Football  

---

## THEORY
In this assignment, two sets of students are represented using linked lists. Linked lists allow flexible insertion and traversal of student records without requiring contiguous memory allocation.

Using linked lists makes it easy to perform set operations such as intersection and symmetric difference. The program identifies students who like both sports, students who like only one sport, and those who like neither by comparing both lists.

---

## PROGRAM CODE
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    string name;
    Node* next;
};

class Set {
public:
    Node* head;
    Set() { head = NULL; }

    void insert(string name) {
        Node* temp = new Node;
        temp->name = name;
        temp->next = NULL;

        if (head == NULL)
            head = temp;
        else {
            Node* p = head;
            while (p->next != NULL)
                p = p->next;
            p->next = temp;
        }
    }

    bool contains(string name) {
        Node* p = head;
        while (p != NULL) {
            if (p->name == name)
                return true;
            p = p->next;
        }
        return false;
    }

    void display() {
        Node* p = head;
        while (p != NULL) {
            cout << p->name << " ";
            p = p->next;
        }
        cout << endl;
    }
};

int main() {
    Set cricket, football;
    int total, n1, n2;
    string name;

    cout << "Enter total number of students: ";
    cin >> total;

    cout << "Enter number of students who like Cricket: ";
    cin >> n1;
    for (int i = 0; i < n1; i++) {
        cin >> name;
        cricket.insert(name);
    }

    cout << "Enter number of students who like Football: ";
    cin >> n2;
    for (int i = 0; i < n2; i++) {
        cin >> name;
        football.insert(name);
    }

    cout << "Students who like both Cricket and Football: ";
    Node* p = cricket.head;
    while (p) {
        if (football.contains(p->name))
            cout << p->name << " ";
        p = p->next;
    }
    cout << endl;

    cout << "Students who like either Cricket or Football but not both: ";
    p = cricket.head;
    while (p) {
        if (!football.contains(p->name))
            cout << p->name << " ";
        p = p->next;
    }
    p = football.head;
    while (p) {
        if (!cricket.contains(p->name))
            cout << p->name << " ";
        p = p->next;
    }
    cout << endl;

    int countCricket = 0, countFootball = 0, countBoth = 0;
    p = cricket.head;
    while (p) { countCricket++; p = p->next; }
    p = football.head;
    while (p) { countFootball++; p = p->next; }

    p = cricket.head;
    while (p) {
        if (football.contains(p->name))
            countBoth++;
        p = p->next;
    }

    int neither = total - (countCricket + countFootball - countBoth);
    cout << "Number of students who like neither Cricket nor Football: " << neither << endl;

    return 0;
}
```

---

## SAMPLE OUTPUT
```
Enter total number of students: 10
Enter number of students who like Cricket: 5
Amit Riya Karan Sita Rahul
Enter number of students who like Football: 4
Karan Riya Neha Pooja

Students who like both Cricket and Football: Riya Karan
Students who like either Cricket or Football but not both: Amit Sita Rahul Neha Pooja
Number of students who like neither Cricket nor Football: 3
```

---

## CONCLUSION
This assignment demonstrates how linked lists can be used to implement set operations efficiently and manage student preferences dynamically.
