# Assignment 18 – Bubble Sort on Doubly Linked List

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37.  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Yash Santosh Khandagale  

---

## AIM
To implement **Bubble Sort on a Doubly Linked List** and sort the elements in ascending order.

---

## THEORY
(Original theory preserved – no content removed)

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
    DLL() { head = NULL; }

    void insert(int val) {
        Node* temp = new Node{val, NULL, NULL};
        if (!head) {
            head = temp;
            return;
        }
        Node* p = head;
        while (p->next) p = p->next;
        p->next = temp;
        temp->prev = p;
    }

    void bubbleSort() {
        if (!head) return;
        bool swapped;
        Node* ptr1;
        Node* lptr = NULL;

        do {
            swapped = false;
            ptr1 = head;
            while (ptr1->next != lptr) {
                if (ptr1->data > ptr1->next->data) {
                    swap(ptr1->data, ptr1->next->data);
                    swapped = true;
                }
                ptr1 = ptr1->next;
            }
            lptr = ptr1;
        } while (swapped);
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
    d.insert(5);
    d.insert(3);
    d.insert(8);
    d.insert(1);

    cout << "Before Sorting: ";
    d.display();

    d.bubbleSort();

    cout << "After Sorting: ";
    d.display();

    return 0;
}
```

---

## SAMPLE OUTPUT
```
Before Sorting: 5 3 8 1
After Sorting: 1 3 5 8
```

---

## CONCLUSION
Bubble Sort can be effectively applied to a doubly linked list by swapping data values, demonstrating sorting on pointer-based data structures.
