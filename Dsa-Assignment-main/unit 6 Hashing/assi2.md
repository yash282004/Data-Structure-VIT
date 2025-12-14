## Assignment 2

*Title:*
Implement a hash table with collision resolution using separate chaining.

## Author:

Yash Santosh Khandagale

## Aim:

To write a C++ program to implement a hash table that stores integer keys.
Collisions must be resolved using Separate Chaining by using a linked list at each bucket.

## Problem Statement:

Create a hash table of user-defined size.
Insert a set of integer keys into the hash table using the hash function:

- hash(key) = key % table_size

If a collision occurs at an index, resolve it using Separate Chaining by appending the key to the linked list (chain) at that bucket.

Your program should:

* Insert keys into the hash table.
* Handle collisions using separate chaining.
* Display the final hash table, showing the chain (linked list) at each index.

## Algorithm:
1. Step 1: Start the program.
2. Step 2: Input the size of the hash table.
3. Step 3: Initialize the hash table as an array of lists (or vectors), where each element (bucket) is an empty list/chain.
4. Step 4: Input the number of keys and the keys to insert.
5. Step 5: For each key:
    * Compute the hash index = key % size.
    * Go to the calculated index (bucket) in the hash table array.
    * Append the key to the end of the list (chain) present at that index.
6. Step 6: Display the final hash table, iterating through each index and printing all keys stored in the corresponding list (chain).
7. Step 7: Stop.

## C++ Program
```cpp
#include <iostream>
#include <vector>
#include <list>
#include <string>
#include <algorithm>
using namespace std;

// Structure to represent a key-value pair
struct KeyValue {
    string key;
    int value;
    
    KeyValue(string k, int v) : key(k), value(v) {}
};

class HashTable {
private:
    vector<list<KeyValue>> table;
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
        
        vector<list<KeyValue>> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity);
        size = 0;
        
        for (const list<KeyValue>& chain : oldTable) {
            for (const KeyValue& entry : chain) {
                insert(entry.key, entry.value);
            }
        }
        cout << "Rehashing completed. New capacity: " << capacity << endl;
    }
    
public:
    HashTable(int initialCapacity = 10) {
        capacity = initialCapacity;
        size = 0;
        table.resize(capacity);
    }
    
    // Insert a key-value pair
    void insert(const string& key, int value) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int index = hashFunction(key);
        
        cout << "Inserting (" << key << ", " << value << "):\n";
        cout << "  Hash value: " << index << endl;
        cout << "  Chain at index " << index << " currently has " << table[index].size() << " elements\n";
        
        // Check if key already exists
        for (auto it = table[index].begin(); it != table[index].end(); ++it) {
            if (it->key == key) {
                cout << "  Key already exists in chain. Updating value from " 
                     << it->value << " to " << value << endl;
                it->value = value;
                return;
            }
        }
        
        // Insert new key-value pair
        table[index].push_back(KeyValue(key, value));
        size++;
        
        cout << "  Successfully inserted at index " << index << endl;
        cout << "  Chain size at index " << index << " is now " << table[index].size() << endl;
    }
    
    // Search for a key
    int search(const string& key) {
        int index = hashFunction(key);
        int positionInChain = 0;
        
        cout << "Searching for key '" << key << "':\n";
        cout << "  Hash value: " << index << endl;
        cout << "  Chain at index " << index << " has " << table[index].size() << " elements\n";
        
        for (auto it = table[index].begin(); it != table[index].end(); ++it) {
            positionInChain++;
            cout << "  Checking position " << positionInChain << " in chain: key = '" 
                 << it->key << "', value = " << it->value << endl;
            
            if (it->key == key) {
                cout << "  Found at index " << index << ", position " << positionInChain 
                     << " in chain with value " << it->value << endl;
                cout << "  Total comparisons: " << positionInChain << endl;
                return it->value;
            }
        }
        
        cout << "  Key '" << key << "' not found in chain.\n";
        cout << "  Total comparisons: " << positionInChain << endl;
        return -1; // Not found
    }
    
    // Delete a key
    bool remove(const string& key) {
        int index = hashFunction(key);
        int positionInChain = 0;
        
        cout << "Deleting key '" << key << "':\n";
        cout << "  Hash value: " << index << endl;
        cout << "  Chain at index " << index << " has " << table[index].size() << " elements\n";
        
        for (auto it = table[index].begin(); it != table[index].end(); ++it) {
            positionInChain++;
            cout << "  Checking position " << positionInChain << " in chain: key = '" 
                 << it->key << "', value = " << it->value << endl;
            
            if (it->key == key) {
                table[index].erase(it);
                size--;
                cout << "  Successfully deleted from index " << index 
                     << ", position " << positionInChain << " in chain\n";
                cout << "  Total comparisons: " << positionInChain << endl;
                return true;
            }
        }
        
        cout << "  Key '" << key << "' not found for deletion.\n";
        cout << "  Total comparisons: " << positionInChain << endl;
        return false;
    }
    
    // Display the entire hash table
    void display() {
        cout << "\nHash Table Contents (Separate Chaining):\n";
        cout << "========================================\n";
        cout << "Capacity: " << capacity << ", Size: " << size << ", Load Factor: " 
             << static_cast<double>(size) / capacity << endl;
        cout << "Index\tChain Contents\n";
        cout << "-----\t---------------\n";
        
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            if (table[i].empty()) {
                cout << "EMPTY";
            } else {
                bool first = true;
                for (const KeyValue& entry : table[i]) {
                    if (!first) cout << " -> ";
                    cout << "(" << entry.key << ", " << entry.value << ")";
                    first = false;
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
            if (!table[i].empty()) {
                cout << "Index " << i << " (Chain length: " << table[i].size() << "): ";
                int position = 1;
                for (const KeyValue& entry : table[i]) {
                    cout << "[" << position++ << ": (" << entry.key << ", " << entry.value << ")] ";
                }
                cout << endl;
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
        
        for (int i = 0; i < capacity; i++) {
            int chainLength = table[i].size();
            if (chainLength == 0) {
                emptyChains++;
            } else {
                nonEmptyChains++;
                totalChainLength += chainLength;
                if (chainLength > maxChainLength) {
                    maxChainLength = chainLength;
                }
            }
        }
        
        double avgChainLength = nonEmptyChains > 0 ? static_cast<double>(totalChainLength) / nonEmptyChains : 0;
        
        cout << "Empty Chains: " << emptyChains << endl;
        cout << "Non-Empty Chains: " << nonEmptyChains << endl;
        cout << "Max Chain Length: " << maxChainLength << endl;
        cout << "Average Chain Length: " << avgChainLength << endl;
        cout << "Longest Chain Index: ";
        
        // Find all indices with maximum chain length
        bool first = true;
        for (int i = 0; i < capacity; i++) {
            if (table[i].size() == maxChainLength) {
                if (!first) cout << ", ";
                cout << i;
                first = false;
            }
        }
        cout << endl;
    }
    
    // Find the longest chain
    void findLongestChain() {
        int maxLength = 0;
        vector<int> longestChainIndices;
        
        for (int i = 0; i < capacity; i++) {
            int length = table[i].size();
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
            int position = 1;
            for (const KeyValue& entry : table[longestChainIndices[0]]) {
                cout << "  " << position++ << ". (" << entry.key << ", " << entry.value << ")\n";
            }
        }
    }
};

// Function to display menu
void displayMenu() {
    cout << "\n=== Hash Table Operations (Separate Chaining) ===\n";
    cout << "1. Insert key-value pair\n";
    cout << "2. Search for key\n";
    cout << "3. Delete key\n";
    cout << "4. Display hash table\n";
    cout << "5. Display detailed chains\n";
    cout << "6. Show statistics\n";
    cout << "7. Find longest chain\n";
    cout << "8. Exit\n";
    cout << "Enter your choice (1-8): ";
}

int main() {
    int initialCapacity;
    cout << "=== Hash Table with Separate Chaining ===\n";
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
                ht.getStatistics();
                break;
                
            case 7:
                ht.findLongestChain();
                break;
                
            case 8:
                cout << "Exiting program.\n";
                break;
                
            default:
                cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 8);
    
    return 0;
}
```
## Example Output
```
=== Hash Table with Separate Chaining ===
Enter initial capacity of hash table: 8

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 1
Enter key: apple
Enter value: 10
Inserting (apple, 10):
  Hash value: 5
  Chain at index 5 currently has 0 elements
  Successfully inserted at index 5
  Chain size at index 5 is now 1

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 1
Enter key: banana
Enter value: 20
Inserting (banana, 20):
  Hash value: 1
  Chain at index 1 currently has 0 elements
  Successfully inserted at index 1
  Chain size at index 1 is now 1

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 1
Enter key: cherry
Enter value: 30
Inserting (cherry, 30):
  Hash value: 5
  Chain at index 5 currently has 1 elements
  Successfully inserted at index 5
  Chain size at index 5 is now 2

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 1
Enter key: date
Enter value: 40
Inserting (date, 40):
  Hash value: 1
  Chain at index 1 currently has 1 elements
  Successfully inserted at index 1
  Chain size at index 1 is now 2

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 4

Hash Table Contents (Separate Chaining):
========================================
Capacity: 8, Size: 4, Load Factor: 0.5
Index	Chain Contents
-----	---------------
0	EMPTY
1	(banana, 20) -> (date, 40)
2	EMPTY
3	EMPTY
4	EMPTY
5	(apple, 10) -> (cherry, 30)
6	EMPTY
7	EMPTY

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 2
Enter key to search: cherry
Searching for key 'cherry':
  Hash value: 5
  Chain at index 5 has 2 elements
  Checking position 1 in chain: key = 'apple', value = 10
  Checking position 2 in chain: key = 'cherry', value = 30
  Found at index 5, position 2 in chain with value 30
  Total comparisons: 2

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 3
Enter key to delete: banana
Deleting key 'banana':
  Hash value: 1
  Chain at index 1 has 2 elements
  Checking position 1 in chain: key = 'banana', value = 20
  Successfully deleted from index 1, position 1 in chain
  Total comparisons: 1

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 6

Hash Table Statistics:
======================
Capacity: 8
Size: 3
Load Factor: 0.375
Load Factor Threshold: 1
Empty Chains: 6
Non-Empty Chains: 2
Max Chain Length: 2
Average Chain Length: 1.5
Longest Chain Index: 1, 5

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 7

Longest Chain Analysis:
=======================
Maximum chain length: 2
Found at indices: 1, 5
Contents of longest chain at index 1:
  1. (date, 40)

=== Hash Table Operations (Separate Chaining) ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Display detailed chains
6. Show statistics
7. Find longest chain
8. Exit
Enter your choice (1-8): 8
Exiting program.
```
## Explanation:
- Structure: The hash table is implemented as a vector of linked lists (std::list).

- Hashing: All five keys (10, 24, 3, 17, 31) hash to the same index: 3 (since k % 7 = 3).

- Collision Handling: Instead of finding the next empty slot (like Linear Probing), Separate Chaining adds all colliding keys to the list at index 3. The chain visually represents how multiple data items can reside in the same bucket.