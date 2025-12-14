# Assignment 2: BST Properties

## Author
**Sejal Dinesh Revanwar**

## Theory 
BST properties help analyze structure: node count gives size, height gives depth, mirror image swaps left and right subtrees.Binary Search Tree properties help analyze the structure and efficiency of the tree.
Counting nodes gives the total number of elements present in the BST.
Height of the BST represents the longest path from root to a leaf node.
Mirror image of a BST is obtained by swapping left and right subtrees recursively.

## AIM
To count total nodes, compute height, and generate mirror image of BST.

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

int countNodes(Node* root) {
    if (!root) return 0;
    return 1 + countNodes(root->left) + countNodes(root->right);
}

int height(Node* root) {
    if (!root) return -1;
    return 1 + max(height(root->left), height(root->right));
}

void mirror(Node* root) {
    if (!root) return;
    swap(root->left, root->right);
    mirror(root->left);
    mirror(root->right);
}

int main() {
    Node* root = NULL;
    root = insert(root, 10);
    root = insert(root, 5);
    root = insert(root, 15);

    cout << "Total Nodes: " << countNodes(root) << endl;
    cout << "Height: " << height(root) << endl;
    mirror(root);
    cout << "Mirror created";
    return 0;
}
```

## Sample Output
```
Total Nodes: 3
Height: 1
Mirror created
```
