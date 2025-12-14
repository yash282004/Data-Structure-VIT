# Assignment 3: Employee Record Search Using Tree

## Author
**Sejal Dinesh Revanwar**

## Theory
Tree data structure allows fast searching of employee records.  
Employee ID is used as key in BST.  
Records are stored in sorted order.  
Inorder traversal displays records in ascending order.  
Efficient for large databases.

## C++ Program
```cpp
#include <iostream>
using namespace std;

struct Node {
    int empId;
    Node* left;
    Node* right;
};

Node* insert(Node* root, int id) {
    if (!root) return new Node{id, NULL, NULL};
    if (id < root->empId)
        root->left = insert(root->left, id);
    else
        root->right = insert(root->right, id);
    return root;
}

void inorder(Node* root) {
    if (!root) return;
    inorder(root->left);
    cout << root->empId << " ";
    inorder(root->right);
}

int main() {
    Node* root = NULL;
    int n, id;
    cout << "Enter number of employees: ";
    cin >> n;

    for (int i = 0; i < n; i++) {
        cin >> id;
        root = insert(root, id);
    }

    inorder(root);
    return 0;
}
```

## Sample Output
```
Enter number of employees: 3
101 105 102
101 102 105
```
