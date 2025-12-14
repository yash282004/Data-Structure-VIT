# Assignment 1: Student Roll Number Assignment Using Tree

## Author
**Yash Santosh Khandagale**

## Theory
A tree data structure is used to assign roll numbers to students based on their previous year’s results.  
The topper is assigned roll number 1, followed by others according to rank.  
Tree structure helps maintain hierarchical ordering of students.  
Insertion is done based on marks or rank comparison.  
Traversal helps display students in proper order.

## C++ Program
```cpp
#include <iostream>
using namespace std;

struct Node {
    int marks;
    Node* left;
    Node* right;
};

Node* insert(Node* root, int marks) {
    if (!root) return new Node{marks, NULL, NULL};
    if (marks > root->marks)
        root->left = insert(root->left, marks);
    else
        root->right = insert(root->right, marks);
    return root;
}

void inorder(Node* root, int &roll) {
    if (!root) return;
    inorder(root->left, roll);
    cout << "Roll No " << roll++ << " -> Marks: " << root->marks << endl;
    inorder(root->right, roll);
}

int main() {
    Node* root = NULL;
    int n, marks;
    cout << "Enter number of students: ";
    cin >> n;

    for (int i = 0; i < n; i++) {
        cout << "Enter marks: ";
        cin >> marks;
        root = insert(root, marks);
    }

    int roll = 1;
    inorder(root, roll);
    return 0;
}
```

## Sample Output
```
Enter number of students: 3
Enter marks: 85
Enter marks: 90
Enter marks: 70
Roll No 1 -> Marks: 90
Roll No 2 -> Marks: 85
Roll No 3 -> Marks: 70
```
