# Assignment 5: Product Deletion Using Search Tree

## Author
**Sejal Dinesh Revanwar**

## Theory
Deletion operation removes outdated products from inventory.  
Products are identified using unique product code.  
BST properties are maintained after deletion.  
Expired products are removed efficiently.  
Ensures accurate inventory data.

## C++ Program
```cpp
#include <iostream>
using namespace std;

struct Node {
    int code;
    Node* left;
    Node* right;
};

Node* insert(Node* root, int code) {
    if (!root) return new Node{code, NULL, NULL};
    if (code < root->code)
        root->left = insert(root->left, code);
    else
        root->right = insert(root->right, code);
    return root;
}

Node* findMin(Node* root) {
    while (root->left) root = root->left;
    return root;
}

Node* deleteNode(Node* root, int key) {
    if (!root) return root;
    if (key < root->code)
        root->left = deleteNode(root->left, key);
    else if (key > root->code)
        root->right = deleteNode(root->right, key);
    else {
        if (!root->left) return root->right;
        if (!root->right) return root->left;
        Node* temp = findMin(root->right);
        root->code = temp->code;
        root->right = deleteNode(root->right, temp->code);
    }
    return root;
}

void inorder(Node* root) {
    if (!root) return;
    inorder(root->left);
    cout << root->code << " ";
    inorder(root->right);
}

int main() {
    Node* root = NULL;
    root = insert(root, 10);
    root = insert(root, 5);
    root = insert(root, 20);

    root = deleteNode(root, 10);
    inorder(root);
    return 0;
}
```

## Sample Output
```
5 20
```
