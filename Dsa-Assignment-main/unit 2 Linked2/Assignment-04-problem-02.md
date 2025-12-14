# Assignment 17 – Polynomial Addition using Singly Linked List

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Vaidehi Vinayak Unhale  
**Roll Number:** 12  
**Division:** SEDA [A]  
**Subject:** Data Structure  
**Assignment No.:** 04  
**Problem Statement:** 02  

---

## AIM
Write a C++ program to perform **addition of two polynomials** using a **singly linked list** representation.

---

## THEORY
A polynomial can be represented as a linked list where each node contains two parts:  
the **coefficient** and the **power** of the variable.

Using a linked list allows efficient traversal and addition of terms by comparing powers.  
Like terms (same power) are added together, while unlike terms are copied as they are.

---

## PROGRAM CODE
```cpp
#include <iostream>
using namespace std;

struct Node {
    int coeff;
    int pow;
    Node* next;
};

class Polynomial {
private:
    Node* head;

public:
    Polynomial() {
        head = NULL;
    }

    void insertTerm(int c, int p) {
        Node* temp = new Node;
        temp->coeff = c;
        temp->pow = p;
        temp->next = NULL;

        if (head == NULL) {
            head = temp;
        } else {
            Node* t = head;
            while (t->next != NULL)
                t = t->next;
            t->next = temp;
        }
    }

    void display() {
        Node* t = head;
        while (t != NULL) {
            cout << t->coeff << "x^" << t->pow;
            if (t->next != NULL)
                cout << " + ";
            t = t->next;
        }
        cout << endl;
    }

    static Polynomial add(Polynomial p1, Polynomial p2) {
        Polynomial result;
        Node* a = p1.head;
        Node* b = p2.head;

        while (a != NULL && b != NULL) {
            if (a->pow == b->pow) {
                result.insertTerm(a->coeff + b->coeff, a->pow);
                a = a->next;
                b = b->next;
            } else if (a->pow > b->pow) {
                result.insertTerm(a->coeff, a->pow);
                a = a->next;
            } else {
                result.insertTerm(b->coeff, b->pow);
                b = b->next;
            }
        }

        while (a != NULL) {
            result.insertTerm(a->coeff, a->pow);
            a = a->next;
        }

        while (b != NULL) {
            result.insertTerm(b->coeff, b->pow);
            b = b->next;
        }

        return result;
    }
};

int main() {
    Polynomial p1, p2, sum;

    // First Polynomial: 5x^2 + 4x^1
    p1.insertTerm(5, 2);
    p1.insertTerm(4, 1);

    // Second Polynomial: 3x^2 + 1x^0
    p2.insertTerm(3, 2);
    p2.insertTerm(1, 0);

    cout << "Polynomial 1: ";
    p1.display();

    cout << "Polynomial 2: ";
    p2.display();

    sum = Polynomial::add(p1, p2);

    cout << "Sum of Polynomials: ";
    sum.display();

    return 0;
}
```

---

## SAMPLE OUTPUT
```
Polynomial 1: 5x^2 + 4x^1
Polynomial 2: 3x^2 + 1x^0
Sum of Polynomials: 8x^2 + 4x^1 + 1x^0
```

---

## CONCLUSION
This program demonstrates how **singly linked lists** can be used to represent and add polynomials efficiently.  
The approach avoids unnecessary memory usage and allows dynamic handling of polynomial terms.
