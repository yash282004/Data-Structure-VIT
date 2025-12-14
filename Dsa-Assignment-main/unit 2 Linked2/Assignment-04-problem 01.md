# Assignment 16 – Set using Generalized Linked List (GLL)

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Vaidehi Vinayak Unhale  
**Roll Number:** 12  
**Division:** SEDA [A]  
**Subject:** Data Structure  
**Assignment No.:** 04  
**Problem Statement:** 01  

---

## AIM
Write a C++ program to implement a **Set using Generalized Linked List (GLL)** and display it in proper set notation.

---

## THEORY
A generalized linked list is an extension of a linked list where each node can store either a data value or a pointer to another linked list.  
This structure is useful for representing hierarchical and nested data such as mathematical sets.

In this assignment, a generalized linked list is used to represent a set that may contain simple elements as well as subsets.

---

## PROGRAM CODE
```cpp
#include <iostream>
#include <string>
using namespace std;

struct GNode {
    int flag;          // 0 = data, 1 = sublist
    string data;
    GNode* down;
    GNode* next;
};

class GLL {
public:
    GNode* head;

    GLL() {
        head = NULL;
    }

    GNode* createList() {
        GNode* first = NULL;
        GNode* last = NULL;
        string item;

        while (true) {
            cin >> item;
            if (item == "end")
                break;

            GNode* node = new GNode;
            node->next = NULL;
            node->down = NULL;

            if (item == "sub") {
                node->flag = 1;
                node->down = createList();
            } else {
                node->flag = 0;
                node->data = item;
            }

            if (first == NULL)
                first = node;
            else
                last->next = node;

            last = node;
        }
        return first;
    }

    void display(GNode* node) {
        cout << "{";
        while (node != NULL) {
            if (node->flag == 0)
                cout << node->data;
            else
                display(node->down);

            if (node->next != NULL)
                cout << ", ";
            node = node->next;
        }
        cout << "}";
    }
};

int main() {
    GLL set;
    cout << "Enter elements (use 'sub' for subset and 'end' to finish): ";
    set.head = set.createList();
    set.display(set.head);
    return 0;
}
```

---

## SAMPLE OUTPUT
```
Input:
p q sub r s t sub u v end w x sub y z end a1 b1 end end

Output:
{p, q, {r, s, t, {u, v}, w, x, {y, z}, a1, b1}}
```

---

## CONCLUSION
This assignment demonstrates the use of a **generalized linked list** to represent complex set structures containing nested subsets.  
The program efficiently stores and displays hierarchical data using recursive traversal.
