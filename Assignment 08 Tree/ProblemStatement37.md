# Assignment 2: BST Operations on Numeric Keys

## Author
**Yash Santosh Khandagale**

## Theory
Binary Search Tree stores elements in sorted order.  
Left subtree contains smaller values and right subtree contains larger values.  
BST supports insert, delete, search, and display operations.  
Menu-driven approach allows user interaction.  
BST improves searching efficiency.

## C++ Program
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
    if (val < root->data)
        root->left = insert(root->left, val);
    else
        root->right = insert(root->right, val);
    return root;
}

void inorder(Node* root) {
    if (!root) return;
    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}

bool search(Node* root, int key) {
    if (!root) return false;
    if (root->data == key) return true;
    if (key < root->data) return search(root->left, key);
    return search(root->right, key);
}

int main() {
    Node* root = NULL;
    int ch, val;
    do {
        cout << "\n1.Insert 2.Search 3.Display 4.Exit: ";
        cin >> ch;
        if (ch == 1) {
            cin >> val;
            root = insert(root, val);
        } else if (ch == 2) {
            cin >> val;
            cout << (search(root, val) ? "Found" : "Not Found") << endl;
        } else if (ch == 3) {
            inorder(root);
            cout << endl;
        }
    } while (ch != 4);
    return 0;
}
```

## Sample Output
```
1.Insert
20
1.Insert
10
3.Display
10 20
2.Search
10
Found
```
