# Assignment 5: Recursive Binary Tree Operations

## Author
**Yash Santosh Khandagale**

## Theory
Recursive traversal naturally follows the structure of a binary tree using function calls.Recursive traversal uses function calls to visit tree nodes based on tree structure.
Inorder traversal visits left subtree, root, and then right subtree.
Preorder traversal visits root before left and right subtrees.
Recursive methods are simple, readable, and closely follow tree hierarchy.

## AIM
To perform inorder, preorder traversal recursively and count leaf nodes.

## C++ Implementation
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

void inorder(Node* root) {
    if (!root) return;
    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}

void preorder(Node* root) {
    if (!root) return;
    cout << root->data << " ";
    preorder(root->left);
    preorder(root->right);
}

int main() {
    Node* root = new Node{10, new Node{5, NULL, NULL}, new Node{15, NULL, NULL}};
    inorder(root);
    cout << endl;
    preorder(root);
    return 0;
}
```

## Sample Output
```
5 10 15
10 5 15
```
