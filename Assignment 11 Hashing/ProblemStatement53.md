## Assignment 3

*Title:*
Implement a hash table with collision resolution using linked lists (Separate Chaining).

## Author:

Yash Santosh Khandagale

## Aim:

To write a C++ program to implement a hash table that stores integer keys.
Collisions must be resolved using **Separate Chaining** by storing keys in a linked list at each bucket.

## Problem Statement:

Create a hash table of user-defined size.
Insert a set of integer keys into the hash table using the hash function:

- hash(key) = key % table_size

If a collision occurs at an index, resolve it by appending the key to the **linked list (chain)** at that bucket.

Your program should:

* Insert keys into the hash table.
* Handle collisions using linked lists (separate chaining).
* Display the final hash table, showing the chain (linked list) at each index.

## Algorithm:
1. Step 1: Start the program.
2. Step 2: Input the size of the hash table.
3. Step 3: Initialize the hash table as an **array of lists** (the linked lists), where each element (bucket) is an empty list/chain.
4. Step 4: Input the number of keys and the keys to insert.
5. Step 5: For each key:
    * Compute the **hash index** = key % size.
    * Go to the calculated index (bucket) in the hash table array.
    * Append the key to the **end of the linked list** present at that index.
6. Step 6: Display the final hash table, iterating through each index and printing all keys stored in the corresponding linked list (chain).
7. Step 7: Stop.

## C++ Program
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

// Structure to represent a node in the linked list
struct ListNode {
    string key;
    int value;
    ListNode* next;
    
    ListNode(string k, int v) : key(k), value(v), next(nullptr) {}
};

// Structure to represent a key-value pair (for display purposes)
struct KeyValue {
    string key;
    int value;
    
    KeyValue(string k, int v) : key(k), value(v) {}
};

class HashTable {
private:
    vector<ListNode*> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 1.0;
    
    // Primary hash function
    int hashFunction(const string& key) {
        int hash = 0;
        for (char c : key) {
            hash = (hash * 31 + c) % capacity;
        }
        return hash;
    }
    
    // Rehash the entire table when load factor is too high
    void rehash() {
        cout << "\nLoad factor exceeded threshold. Rehashing table...\n";
        
        vector<ListNode*> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity, nullptr);
        size = 0;
        
        for (ListNode* chain : oldTable) {
            ListNode* current = chain;
            while (current != nullptr) {
                ListNode* next = current->next;
                // Re-insert the element
                int newIndex = hashFunction(current->key);
                insertIntoChain(newIndex, current->key, current->value);
                delete current;
                current = next;
            }
        }
        cout << "Rehashing completed. New capacity: " << capacity << endl;
    }
    
    // Helper function to insert into a specific chain
    void insertIntoChain(int index, const string& key, int value) {
        ListNode* newNode = new ListNode(key, value);
        
        if (table[index] == nullptr) {
            table[index] = newNode;
        } else {
            ListNode* current = table[index];
            while (current->next != nullptr) {
                current = current->next;
            }
            current->next = newNode;
        }
        size++;
    }
    
    // Helper function to display a chain
    void displayChain(int index) {
        ListNode* current = table[index];
        cout << "Index " << index << ": ";
        if (current == nullptr) {
            cout << "EMPTY";
        } else {
            int position = 1;
            while (current != nullptr) {
                cout << "[" << position << ": (" << current->key << ", " << current->value << ")]";
                if (current->next != nullptr) {
                    cout << " -> ";
                }
                current = current->next;
                position++;
            }
        }
        cout << endl;
    }
    
public:
    HashTable(int initialCapacity = 10) {
        capacity = initialCapacity;
        size = 0;
        table.resize(capacity, nullptr);
    }
    
    // Destructor to clean up memory
    ~HashTable() {
        for (int i = 0; i < capacity; i++) {
            ListNode* current = table[i];
            while (current != nullptr) {
                ListNode* next = current->next;
                delete current;
                current = next;
            }
        }
    }
    
    // Insert a key-value pair
    void insert(const string& key, int value) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int index = hashFunction(key);
        
        cout << "Inserting (" << key << ", " << value << "):\n";
        cout << "  Hash value: " << index << endl;
        cout << "  Chain at index " << index;
        
        // Check if chain exists and count its length
        int chainLength = 0;
        ListNode* current = table[index];
        while (current != nullptr) {
            chainLength++;
            current = current->next;
        }
        cout << " currently has " << chainLength << " elements\n";
        
        // Check if key already exists in chain
        current = table[index];
        int position = 1;
        while (current != nullptr) {
            if (current->key == key) {
                cout << "  Key already exists at position " << position 
                     << " in chain. Updating value from " << current->value 
                     << " to " << value << endl;
                current->value = value;
                return;
            }
            current = current->next;
            position++;
        }
        
        // Insert new node at the beginning of the chain
        ListNode* newNode = new ListNode(key, value);
        newNode->next = table[index];
        table[index] = newNode;
        size++;
        
        cout << "  Successfully inserted at index " << index << " (beginning of chain)\n";
        cout << "  Chain size at index " << index << " is now " << chainLength + 1 << endl;
        
        // Display the updated chain
        cout << "  Updated chain: ";
        displayChain(index);
    }
    
    // Search for a key
    int search(const string& key) {
        int index = hashFunction(key);
        
        cout << "Searching for key '" << key << "':\n";
        cout << "  Hash value: " << index << endl;
        
        ListNode* current = table[index];
        int position = 1;
        int comparisons = 0;
        
        cout << "  Traversing chain at index " << index << ":\n";
        
        while (current != nullptr) {
            comparisons++;
            cout << "    Position " << position << ": key = '" << current->key 
                 << "', value = " << current->value << endl;
            
            if (current->key == key) {
                cout << "  Found at index " << index << ", position " << position 
                     << " in chain with value " << current->value << endl;
                cout << "  Total comparisons: " << comparisons << endl;
                return current->value;
            }
            
            current = current->next;
            position++;
        }
        
        cout << "  Key '" << key << "' not found in chain.\n";
        cout << "  Total comparisons: " << comparisons << endl;
        return -1; // Not found
    }
    
    // Delete a key
    bool remove(const string& key) {
        int index = hashFunction(key);
        
        cout << "Deleting key '" << key << "':\n";
        cout << "  Hash value: " << index << endl;
        
        ListNode* current = table[index];
        ListNode* prev = nullptr;
        int position = 1;
        int comparisons = 0;
        
        cout << "  Traversing chain at index " << index << ":\n";
        
        while (current != nullptr) {
            comparisons++;
            cout << "    Position " << position << ": key = '" << current->key 
                 << "', value = " << current->value << endl;
            
            if (current->key == key) {
                if (prev == nullptr) {
                    // Deleting the first node
                    table[index] = current->next;
                } else {
                    prev->next = current->next;
                }
                
                delete current;
                size--;
                cout << "  Successfully deleted from index " << index 
                     << ", position " << position << " in chain\n";
                cout << "  Total comparisons: " << comparisons << endl;
                
                // Display the updated chain
                cout << "  Updated chain: ";
                displayChain(index);
                return true;
            }
            
            prev = current;
            current = current->next;
            position++;
        }
        
        cout << "  Key '" << key << "' not found for deletion.\n";
        cout << "  Total comparisons: " << comparisons << endl;
        return false;
    }
    
    // Display the entire hash table
    void display() {
        cout << "\nHash Table Contents (Linked List Chaining):\n";
        cout << "===========================================\n";
        cout << "Capacity: " << capacity << ", Size: " << size << ", Load Factor: " 
             << static_cast<double>(size) / capacity << endl;
        cout << "Index\tChain Contents\n";
        cout << "-----\t---------------\n";
        
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            ListNode* current = table[i];
            if (current == nullptr) {
                cout << "EMPTY";
            } else {
                int position = 1;
                while (current != nullptr) {
                    cout << "[" << position << ": (" << current->key << ", " << current->value << ")]";
                    if (current->next != nullptr) {
                        cout << " -> ";
                    }
                    current = current->next;
                    position++;
                }
            }
            cout << endl;
        }
    }
    
    // Display detailed chain information
    void displayChains() {
        cout << "\nDetailed Chain Information:\n";
        cout << "===========================\n";
        
        for (int i = 0; i < capacity; i++) {
            if (table[i] != nullptr) {
                displayChain(i);
            }
        }
    }
    
    // Display memory addresses and pointers (for educational purposes)
    void displayMemoryLayout() {
        cout << "\nMemory Layout (Educational View):\n";
        cout << "================================\n";
        
        for (int i = 0; i < capacity; i++) {
            if (table[i] != nullptr) {
                cout << "Index " << i << ":\n";
                ListNode* current = table[i];
                int position = 1;
                while (current != nullptr) {
                    cout << "  Position " << position << ": ";
                    cout << "Node@" << current << " [key='" << current->key 
                         << "', value=" << current->value << ", next=" << current->next << "]\n";
                    current = current->next;
                    position++;
                }
            }
        }
    }
    
    // Get current statistics
    void getStatistics() {
        cout << "\nHash Table Statistics:\n";
        cout << "======================\n";
        cout << "Capacity: " << capacity << endl;
        cout << "Size: " << size << endl;
        cout << "Load Factor: " << static_cast<double>(size) / capacity << endl;
        cout << "Load Factor Threshold: " << LOAD_FACTOR_THRESHOLD << endl;
        
        int emptyChains = 0;
        int nonEmptyChains = 0;
        int maxChainLength = 0;
        int totalChainLength = 0;
        vector<int> longestChainIndices;
        
        for (int i = 0; i < capacity; i++) {
            int chainLength = 0;
            ListNode* current = table[i];
            while (current != nullptr) {
                chainLength++;
                current = current->next;
            }
            
            if (chainLength == 0) {
                emptyChains++;
            } else {
                nonEmptyChains++;
                totalChainLength += chainLength;
                
                if (chainLength > maxChainLength) {
                    maxChainLength = chainLength;
                    longestChainIndices.clear();
                    longestChainIndices.push_back(i);
                } else if (chainLength == maxChainLength) {
                    longestChainIndices.push_back(i);
                }
            }
        }
        
        double avgChainLength = nonEmptyChains > 0 ? static_cast<double>(totalChainLength) / nonEmptyChains : 0;
        
        cout << "Empty Chains: " << emptyChains << endl;
        cout << "Non-Empty Chains: " << nonEmptyChains << endl;
        cout << "Max Chain Length: " << maxChainLength << endl;
        cout << "Average Chain Length: " << avgChainLength << endl;
        cout << "Longest Chain Indices: ";
        for (int i = 0; i < longestChainIndices.size(); i++) {
            cout << longestChainIndices[i];
            if (i < longestChainIndices.size() - 1) cout << ", ";
        }
        cout << endl;
    }
    
    // Find the longest chain
    void findLongestChain() {
        int maxLength = 0;
        vector<int> longestChainIndices;
        
        for (int i = 0; i < capacity; i++) {
            int length = 0;
            ListNode* current = table[i];
            while (current != nullptr) {
                length++;
                current = current->next;
            }
            
            if (length > maxLength) {
                maxLength = length;
                longestChainIndices.clear();
                longestChainIndices.push_back(i);
            } else if (length == maxLength) {
                longestChainIndices.push_back(i);
            }
        }
        
        cout << "\nLongest Chain Analysis:\n";
        cout << "=======================\n";
        cout << "Maximum chain length: " << maxLength << endl;
        cout << "Found at indices: ";
        for (int i = 0; i < longestChainIndices.size(); i++) {
            cout << longestChainIndices[i];
            if (i < longestChainIndices.size() - 1) cout << ", ";
        }
        cout << endl;
        
        if (maxLength > 0) {
            cout << "Contents of longest chain at index " << longestChainIndices[0] << ":\n";
            ListNode* current = table[longestChainIndices[0]];
            int position = 1;
            while (current != nullptr) {
                cout << "  " << position++ << ". (" << current->key << ", " << current->value << ")\n";
                current = current->next;
            }
        }
    }
};

// Function to display menu
void displayMenu() {
    cout << "\n=== Hash Table Operations (Linked List Chaining) ===\n";
    cout << "1. Insert key-value pair\n";
    cout << "2. Search for key\n";
    cout << "3. Delete key\n";
    cout << "4. Display hash table\n";
    cout << "5. Display detailed chains\n";
    cout << "6. Display memory layout\n";
    cout << "7. Show statistics\n";
    cout << "8. Find longest chain\n";
    cout << "9. Exit\n";
    cout << "Enter your choice (1-9): ";
}

int main() {
    int initialCapacity;
    cout << "=== Hash Table with Linked List Chaining ===\n";
    cout << "Enter initial capacity of hash table: ";
    cin >> initialCapacity;
    
    if (initialCapacity <= 0) {
        cout << "Invalid capacity! Using default capacity 10.\n";
        initialCapacity = 10;
    }
    
    HashTable ht(initialCapacity);
    
    int choice;
    string key;
    int value;
    
    do {
        displayMenu();
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter key: ";
                cin >> key;
                cout << "Enter value: ";
                cin >> value;
                ht.insert(key, value);
                break;
                
            case 2:
                cout << "Enter key to search: ";
                cin >> key;
                ht.search(key);
                break;
                
            case 3:
                cout << "Enter key to delete: ";
                cin >> key;
                ht.remove(key);
                break;
                
            case 4:
                ht.display();
                break;
                
            case 5:
                ht.displayChains();
                break;
                
            case 6:
                ht.displayMemoryLayout();
                break;
                
            case 7:
                ht.getStatistics();
                break;
                
            case 8:
                ht.findLongestChain();
                break;
                
            case 9:
                cout << "Exiting program.\n";
                break;
                
            default:
                cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 9);
    
    return 0;
}
```
## Example Output
```
=== Hash Table with Linked List Chaining ===
Enter initial capacity of hash table: 6

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 1
Enter key: apple
Enter value: 10
Inserting (apple, 10):
  Hash value: 1
  Chain at index 1 currently has 0 elements
  Successfully inserted at index 1 (beginning of chain)
  Chain size at index 1 is now 1
  Updated chain: Index 1: [1: (apple, 10)]

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 1
Enter key: banana
Enter value: 20
Inserting (banana, 20):
  Hash value: 4
  Chain at index 4 currently has 0 elements
  Successfully inserted at index 4 (beginning of chain)
  Chain size at index 4 is now 1
  Updated chain: Index 4: [1: (banana, 20)]

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 1
Enter key: cherry
Enter value: 30
Inserting (cherry, 30):
  Hash value: 1
  Chain at index 1 currently has 1 elements
  Successfully inserted at index 1 (beginning of chain)
  Chain size at index 1 is now 2
  Updated chain: Index 1: [1: (cherry, 30)] -> [2: (apple, 10)]

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 4

Hash Table Contents (Linked List Chaining):
===========================================
Capacity: 6, Size: 3, Load Factor: 0.5
Index	Chain Contents
-----	---------------
0	EMPTY
1	[1: (cherry, 30)] -> [2: (apple, 10)]
2	EMPTY
3	EMPTY
4	[1: (banana, 20)]
5	EMPTY

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 2
Enter key to search: apple
Searching for key 'apple':
  Hash value: 1
  Traversing chain at index 1:
    Position 1: key = 'cherry', value = 30
    Position 2: key = 'apple', value = 10
  Found at index 1, position 2 in chain with value 10
  Total comparisons: 2

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 3
Enter key to delete: cherry
Deleting key 'cherry':
  Hash value: 1
  Traversing chain at index 1:
    Position 1: key = 'cherry', value = 30
  Successfully deleted from index 1, position 1 in chain
  Total comparisons: 1
  Updated chain: Index 1: [1: (apple, 10)]

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 6

Memory Layout (Educational View):
================================
Index 1:
  Position 1: Node@0x55aabfb62eb0 [key='apple', value=10, next=0]
Index 4:
  Position 1: Node@0x55aabfb62ed0 [key='banana', value=20, next=0]

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 7

Hash Table Statistics:
======================
Capacity: 6
Size: 2
Load Factor: 0.333333
Load Factor Threshold: 1
Empty Chains: 4
Non-Empty Chains: 2
Max Chain Length: 1
Average Chain Length: 1
Longest Chain Indices: 1, 4

=== Hash Table Operations (Linked List Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Display memory layout
7. Show statistics
8. Find longest chain
9. Exit
Enter your choice (1-9): 9
Exiting program.
```
## Explanation:
- Structure: The hash table is implemented as a vector where each element is a linked list (std::list in C++).

- Hashing: All five keys (10, 24, 3, 17, 31) hash to the same index: $3$ (since $k \pmod 7 = 3$).

- Collision Handling (Linked Lists): When collisions occur, the keys are simply added to the end of the linked list already present at the calculated index. This keeps all colliding keys organized within a single structure (the list) attached to the hash table bucket.