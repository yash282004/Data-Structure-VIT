# Assignment 1: Binary Search Tree Operations

## Author
**Yash Santosh Khandagale**

## Theory 
A Binary Search Tree (BST) is a binary tree where the left subtree contains values smaller than the root and the right subtree contains values greater than the root. BST allows efficient searching, insertion, and deletion.
A Binary Search Tree (BST) is a hierarchical data structure in which each node has at most two children.
The left subtree contains values smaller than the root node, while the right subtree contains larger values.
BST supports efficient insertion, deletion, and searching operations.
Level-wise traversal displays nodes level by level starting from the root.

## AIM
To perform BST operations: Create, Insert, Delete, and Level-wise Display.

## C++ Implementation
```cpp
#include <iostream>
#include <queue>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

Node* insert(Node* root, int val) {
    if (!root) {
        Node* temp = new Node{val, NULL, NULL};
        return temp;
    }
    if (val < root->data)
        root->left = insert(root->left, val);
    else
        root->right = insert(root->right, val);
    return root;
}

void levelOrder(Node* root) {
    if (!root) return;
    queue<Node*> q;
    q.push(root);
    while (!q.empty()) {
        Node* temp = q.front(); q.pop();
        cout << temp->data << " ";
        if (temp->left) q.push(temp->left);
        if (temp->right) q.push(temp->right);
    }
}

int main() {
    Node* root = NULL;
    int ch, val;
    do {
        cout << "\n1.Insert 2.Display(Level) 3.Exit\n";
        cin >> ch;
        if (ch == 1) {
            cout << "Enter value: ";
            cin >> val;
            root = insert(root, val);
        } else if (ch == 2) {
            levelOrder(root);
        }
    } while (ch != 3);
    return 0;
}
```

## Sample Output
```
Enter value: 50
Enter value: 30
Enter value: 70
50 30 70
```
