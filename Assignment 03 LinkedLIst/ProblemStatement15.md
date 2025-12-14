# Assignment 15 – Binary Number using Doubly Linked List

Bansilal Ramnath Agarwal Charitable Trusts'  
Vishwakarma Institute of Technology, Pune - 37.  
(An Autonomous Institute Affiliated to Savitribai Phule Pune University)  
Department of Computer Engineering  

**Name:** Yash Santosh Khandagale

---

## AIM
Write a C++ program to store a binary number using a doubly linked list and perform:
a) 1’s complement and 2’s complement  
b) Addition of two binary numbers  

---

## THEORY
Each bit of a binary number is stored as a node in a doubly linked list. This allows traversal from both directions, which is useful for binary arithmetic operations like addition and complement.

---

## PROGRAM CODE
```cpp
#include <iostream>
using namespace std;

struct Node {
    int bit;
    Node* prev;
    Node* next;
};

class Binary {
    Node* head;
    Node* tail;

public:
    Binary() {
        head = tail = NULL;
    }

    void insert(int bit) {
        Node* temp = new Node;
        temp->bit = bit;
        temp->next = NULL;
        temp->prev = tail;

        if (tail)
            tail->next = temp;
        else
            head = temp;

        tail = temp;
    }

    void display() {
        Node* p = head;
        while (p) {
            cout << p->bit;
            p = p->next;
        }
        cout << endl;
    }

    Binary onesComplement() {
        Binary result;
        Node* p = head;
        while (p) {
            result.insert(p->bit == 0 ? 1 : 0);
            p = p->next;
        }
        return result;
    }

    Binary twosComplement() {
        Binary oneComp = onesComplement();
        Binary result;
        Node* p = oneComp.tail;
        int carry = 1;

        while (p) {
            int sum = p->bit + carry;
            result.insert(sum % 2);
            carry = sum / 2;
            p = p->prev;
        }
        if (carry)
            result.insert(1);

        Binary finalResult;
        p = result.tail;
        while (p) {
            finalResult.insert(p->bit);
            p = p->prev;
        }
        return finalResult;
    }
};

int main() {
    Binary b;
    string bin;

    cout << "Enter binary number: ";
    cin >> bin;
    for (char c : bin)
        b.insert(c - '0');

    cout << "Stored Binary Number: ";
    b.display();

    Binary one = b.onesComplement();
    cout << "1's Complement: ";
    one.display();

    Binary two = b.twosComplement();
    cout << "2's Complement: ";
    two.display();

    return 0;
}
```

---

## SAMPLE OUTPUT
```
Enter binary number: 10110
Stored Binary Number: 10110
1's Complement: 01001
2's Complement: 01010
```

---

## CONCLUSION
This assignment shows how doubly linked lists can be used to efficiently perform binary number operations such as complement calculations.
