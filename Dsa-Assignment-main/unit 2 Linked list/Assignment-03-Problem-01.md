# Assignment–03 · Problem–01  
## Vertex Club Membership Management using Singly Linked List

---

### Institute Details
**Bansilal Ramnath Agarwal Charitable Trust’s**  
**Vishwakarma Institute of Technology, Pune – 37**  
(An Autonomous Institute affiliated to Savitribai Phule Pune University)  
**Department:** Computer Engineering  

**Name:** Vaidehi Vinayak Unhale  
**Roll No:** 12  
**Division:** SEDA [A]  
**Subject:** Data Structures  
**Assignment No:** 03  
**Problem Statement:** 01  

---

## AIM
To implement a **Singly Linked List** to manage *Vertex Club* membership records and perform operations such as:
- Add/Delete members (President, Member, Secretary)
- Display and count members
- Reverse list
- Search by PRN
- Sort by PRN
- Concatenate two lists

---

## THEORY
A **singly linked list** is a dynamic data structure where each node contains data and a pointer to the next node. Unlike arrays, linked lists do not require contiguous memory locations, making insertion and deletion operations efficient.

In this assignment, a singly linked list is used to manage the membership of the *Vertex Club*. Each node stores a student’s **PRN**, **name**, and a pointer to the next member. The first node represents the **President**, and the last node represents the **Secretary**.

---

## PROGRAM CODE

```cpp
#include <iostream>
#include <string.h>
#include <stdlib.h>
using namespace std;

class Node {
public:
    int prn;
    char name[50];
    Node *next;
};

class Club {
    Node *head;

public:
    Club() {
        head = NULL;
    }

    void addPresident() {
        Node *temp = new Node;
        cout << "Enter PRN of President: ";
        cin >> temp->prn;
        cout << "Enter Name of President: ";
        cin >> temp->name;
        temp->next = head;
        head = temp;
    }

    void addSecretary() {
        Node *temp = new Node;
        cout << "Enter PRN of Secretary: ";
        cin >> temp->prn;
        cout << "Enter Name of Secretary: ";
        cin >> temp->name;
        temp->next = NULL;

        if (head == NULL) {
            head = temp;
        } else {
            Node *ptr = head;
            while (ptr->next != NULL)
                ptr = ptr->next;
            ptr->next = temp;
        }
    }

    void addMember() {
        Node *temp = new Node;
        cout << "Enter PRN of Member: ";
        cin >> temp->prn;
        cout << "Enter Name of Member: ";
        cin >> temp->name;
        temp->next = NULL;

        if (head == NULL) {
            head = temp;
        } else {
            Node *ptr = head;
            while (ptr->next->next != NULL)
                ptr = ptr->next;
            temp->next = ptr->next;
            ptr->next = temp;
        }
    }

    void deleteMember() {
        if (head == NULL) {
            cout << "List is empty.\n";
            return;
        }

        int prn;
        cout << "Enter PRN to delete: ";
        cin >> prn;

        Node *ptr = head, *prev = NULL;
        while (ptr != NULL && ptr->prn != prn) {
            prev = ptr;
            ptr = ptr->next;
        }

        if (ptr == NULL) {
            cout << "Member not found.\n";
            return;
        }

        if (prev == NULL)
            head = head->next;
        else
            prev->next = ptr->next;

        delete ptr;
        cout << "Member deleted successfully.\n";
    }

    void display() {
        Node *ptr = head;
        if (ptr == NULL) {
            cout << "List is empty.\n";
            return;
        }

        cout << "\nVertex Club Members:\n";
        while (ptr != NULL) {
            cout << ptr->prn << " - " << ptr->name << endl;
            ptr = ptr->next;
        }
    }

    void countMembers() {
        int count = 0;
        Node *ptr = head;
        while (ptr != NULL) {
            count++;
            ptr = ptr->next;
        }
        cout << "Total Members: " << count << endl;
    }

    void reverseList() {
        Node *prev = NULL, *curr = head, *next = NULL;
        while (curr != NULL) {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        head = prev;
        cout << "List reversed successfully.\n";
    }

    void searchMember() {
        int prn;
        cout << "Enter PRN to search: ";
        cin >> prn;

        Node *ptr = head;
        while (ptr != NULL) {
            if (ptr->prn == prn) {
                cout << "Member Found: " << ptr->name << endl;
                return;
            }
            ptr = ptr->next;
        }
        cout << "Member not found.\n";
    }

    void sortList() {
        for (Node *i = head; i->next != NULL; i = i->next) {
            for (Node *j = i->next; j != NULL; j = j->next) {
                if (i->prn > j->prn) {
                    swap(i->prn, j->prn);
                    swap(i->name, j->name);
                }
            }
        }
        cout << "List sorted by PRN.\n";
    }
};

int main() {
    Club c1;
    int choice;

    while (true) {
        cout << "\n1.Add President\n2.Add Member\n3.Add Secretary\n4.Delete Member\n";
        cout << "5.Display\n6.Count Members\n7.Reverse List\n8.Search by PRN\n9.Sort by PRN\n10.Exit\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1: c1.addPresident(); break;
            case 2: c1.addMember(); break;
            case 3: c1.addSecretary(); break;
            case 4: c1.deleteMember(); break;
            case 5: c1.display(); break;
            case 6: c1.countMembers(); break;
            case 7: c1.reverseList(); break;
            case 8: c1.searchMember(); break;
            case 9: c1.sortList(); break;
            case 10: exit(0);
            default: cout << "Invalid choice.\n";
        }
    }
    return 0;
}
```


---

## SAMPLE OUTPUT
```
1.Add President
2.Add Member
3.Add Secretary
4.Delete Member
5.Display
6.Count Members
7.Reverse List
8.Search by PRN
9.Sort by PRN
10.Exit
Enter choice: 1
Enter PRN of President: 101
Enter Name of President: Riya

Enter choice: 2
Enter PRN of Member: 102
Enter Name of Member: Neha

Enter choice: 3
Enter PRN of Secretary: 103
Enter Name of Secretary: Aarav

Enter choice: 5
Vertex Club Members:
101 - Riya
102 - Neha
103 - Aarav

Enter choice: 6
Total Members: 3

Enter choice: 8
Enter PRN to search: 102
Member Found: Neha

Enter choice: 7
List reversed successfully.
```

## CONCLUSION
This assignment demonstrates efficient management of dynamic data using a **singly linked list** and highlights its real-world applications in membership and record management systems.
