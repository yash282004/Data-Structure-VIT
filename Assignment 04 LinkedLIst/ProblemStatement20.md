# Assignment 20 – Front and Back Split of Singly Linked List

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37.  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Yash Santosh Khandagale

---

## AIM
To write a C++ program to **split a singly linked list into two halves** using the **front–back split** technique.

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
    Node* next;
};

class LinkedList {
    Node* head;

public:
    LinkedList() {
        head = NULL;
    }

    void insertEnd(int val) {
        Node* temp = new Node{val, NULL};
        if (!head) {
            head = temp;
            return;
        }
        Node* p = head;
        while (p->next)
            p = p->next;
        p->next = temp;
    }

    void frontBackSplit(Node** frontRef, Node** backRef) {
        if (!head || !head->next) {
            *frontRef = head;
            *backRef = NULL;
            return;
        }

        Node* slow = head;
        Node* fast = head->next;

        while (fast) {
            fast = fast->next;
            if (fast) {
                slow = slow->next;
                fast = fast->next;
            }
        }

        *frontRef = head;
        *backRef = slow->next;
        slow->next = NULL;
    }

    static void display(Node* node) {
        while (node) {
            cout << node->data << " ";
            node = node->next;
        }
        cout << endl;
    }
};

int main() {
    LinkedList l;
    l.insertEnd(10);
    l.insertEnd(20);
    l.insertEnd(30);
    l.insertEnd(40);
    l.insertEnd(50);

    Node* front = NULL;
    Node* back = NULL;

    l.frontBackSplit(&front, &back);

    cout << "Front list: ";
    LinkedList::display(front);

    cout << "Back list: ";
    LinkedList::display(back);

    return 0;
}
```

---

## SAMPLE OUTPUT
```
Front list: 10 20 30
Back list: 40 50
```

---

## CONCLUSION
The front–back split algorithm efficiently divides a linked list into two halves using the slow and fast pointer technique.
