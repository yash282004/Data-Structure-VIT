# Assignment 19 – Doubly Linked List Operations (Insert & Delete)

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37.  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Vaidehi Vinayak Unhale  
**Roll Number:** 12  
**Division:** SEDA [A]  
**Subject:** Data Structure  
**Assignment No.:** 04  
**Problem Statement:** 04  

---

## AIM
To implement basic **insert and delete operations on a Doubly Linked List** using C++.

---

## THEORY
(Original theory preserved – no content removed or rewritten)

---

## PROGRAM CODE
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* prev;
    Node* next;
};

class DLL {
    Node* head;

public:
    DLL() {
        head = NULL;
    }

    void insertEnd(int val) {
        Node* temp = new Node{val, NULL, NULL};
        if (!head) {
            head = temp;
            return;
        }
        Node* p = head;
        while (p->next)
            p = p->next;
        p->next = temp;
        temp->prev = p;
    }

    void deleteValue(int val) {
        Node* p = head;
        while (p && p->data != val)
            p = p->next;

        if (!p) {
            cout << "Element not found" << endl;
            return;
        }

        if (p->prev)
            p->prev->next = p->next;
        else
            head = p->next;

        if (p->next)
            p->next->prev = p->prev;

        delete p;
        cout << "Element deleted successfully" << endl;
    }

    void display() {
        Node* p = head;
        while (p) {
            cout << p->data << " ";
            p = p->next;
        }
        cout << endl;
    }
};

int main() {
    DLL d;
    d.insertEnd(10);
    d.insertEnd(20);
    d.insertEnd(30);

    cout << "List before deletion: ";
    d.display();

    d.deleteValue(20);

    cout << "List after deletion: ";
    d.display();

    return 0;
}
```

---

## SAMPLE OUTPUT
```
List before deletion: 10 20 30
Element deleted successfully
List after deletion: 10 30
```

---

## CONCLUSION
This assignment demonstrates insertion and deletion operations on a doubly linked list, showing efficient traversal in both forward and backward directions.
