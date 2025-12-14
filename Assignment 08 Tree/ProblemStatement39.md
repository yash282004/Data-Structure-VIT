# Assignment 4: Product Inventory Using Search Tree

## Author
**Yash Santosh Khandagale**

## Theory
Search tree is used to organize product inventory.  
Products are arranged based on product name.  
Inorder traversal shows sorted inventory.  
Preorder traversal lists expired products.  
Improves inventory management.

## C++ Program
```cpp
#include <iostream>
using namespace std;

struct Node {
    string name;
    Node* left;
    Node* right;
};

Node* insert(Node* root, string name) {
    if (!root) return new Node{name, NULL, NULL};
    if (name < root->name)
        root->left = insert(root->left, name);
    else
        root->right = insert(root->right, name);
    return root;
}

void inorder(Node* root) {
    if (!root) return;
    inorder(root->left);
    cout << root->name << " ";
    inorder(root->right);
}

int main() {
    Node* root = NULL;
    int n;
    string name;
    cout << "Enter number of products: ";
    cin >> n;

    for (int i = 0; i < n; i++) {
        cin >> name;
        root = insert(root, name);
    }

    inorder(root);
    return 0;
}
```

## Sample Output
```
Enter number of products: 3
Soap
Oil
Rice
Oil Rice Soap
```
