# Assignment 4: Non-Recursive Binary Tree Operations

## Author
**Yash Santosh Khandagale**

## Theory 
Non-recursive tree traversal is performed using a stack instead of function recursion.
It avoids overhead caused by recursive function calls.
Inorder and preorder traversals can be implemented efficiently using stacks.
This method is useful when recursion is restricted or stack control is required.

## AIM
To perform inorder, preorder traversal non-recursively and count leaf nodes.

## C++ Implementation
```cpp
#include <iostream>
#include <stack>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

void inorder(Node* root) {
    stack<Node*> s;
    Node* curr = root;
    while (curr || !s.empty()) {
        while (curr) {
            s.push(curr);
            curr = curr->left;
        }
        curr = s.top(); s.pop();
        cout << curr->data << " ";
        curr = curr->right;
    }
}

int main() {
    Node* root = new Node{10, new Node{5, NULL, NULL}, new Node{15, NULL, NULL}};
    inorder(root);
    return 0;
}
```

## Sample Output
```
5 10 15
```
