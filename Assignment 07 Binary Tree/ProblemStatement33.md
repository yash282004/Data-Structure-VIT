# Assignment 3: Minimum and Maximum in BST

## Author
**Yash Santosh Khandagale**

## Theory 
In a BST, the minimum value lies in the leftmost node and maximum value lies in the rightmost node.In a Binary Search Tree, elements are arranged in sorted order.
The minimum value is found at the leftmost node of the tree.
The maximum value is found at the rightmost node of the tree.
This property allows quick retrieval of minimum and maximum elements.

## AIM
To find minimum and maximum element in a BST.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

Node* insert(Node* root, int val) {
    if (!root) return new Node{val, NULL, NULL};
    if (val < root->data) root->left = insert(root->left, val);
    else root->right = insert(root->right, val);
    return root;
}

int findMin(Node* root) {
    while (root->left)
        root = root->left;
    return root->data;
}

int findMax(Node* root) {
    while (root->right)
        root = root->right;
    return root->data;
}

int main() {
    Node* root = NULL;
    root = insert(root, 40);
    root = insert(root, 20);
    root = insert(root, 60);

    cout << "Min: " << findMin(root) << endl;
    cout << "Max: " << findMax(root) << endl;
    return 0;
}
```

## Sample Output
```
Min: 20
Max: 60
```
