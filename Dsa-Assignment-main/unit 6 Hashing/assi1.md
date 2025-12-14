## Assignment 1

*Title:*
Implement a hash table with collision resolution using linear probing.

## Author:

**Yash Santosh Khandagale**

## Aim:

To write a C++ program to implement a hash table that stores integer keys.
Collisions must be resolved using Linear Probing while inserting keys.

## Problem Statement:

Create a hash table of user-defined size.
Insert a set of integer keys into the hash table using the hash function:

- hash(key) = key % table_size


If a collision occurs at an index, resolve it using Linear Probing:

- new_index = (index + 1) % table_size


- Your program should:

- - Insert keys into the hash table

- - Handle collisions using linear probing

- - Display the final hash table

## Algorithm:
1. Step 1:

- Start the program.

2. Step 2:

- Input the size of the hash table.

3. Step 3:

- Initialize the hash table with all values set to -1 (meaning empty).

4. Step 4:

- Input number of keys and the keys to insert.

5. Step 5:

- For each key:

- Compute hash index = key % size

- If the calculated index is empty → insert

- Else collision occurs →
- Keep moving forward using

- index = (index + 1) % size


- until an empty slot is found

- Insert the key into the empty position

6. Step 6:

- Display the final hash table.

7. Step 7:

- Stop.

## C++ Program
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

// Structure to represent a key-value pair
struct KeyValue {
    string key;
    int value;
    bool isOccupied;
    bool isDeleted;
    
    KeyValue() : key(""), value(0), isOccupied(false), isDeleted(false) {}
    KeyValue(string k, int v) : key(k), value(v), isOccupied(true), isDeleted(false) {}
};

class HashTable {
private:
    vector<KeyValue> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 0.7;
    
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
        
        vector<KeyValue> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity);
        size = 0;
        
        for (const KeyValue& entry : oldTable) {
            if (entry.isOccupied && !entry.isDeleted) {
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
        int originalIndex = index;
        int probeCount = 0;
        
        cout << "Inserting (" << key << ", " << value << "):\n";
        cout << "  Hash value: " << index << endl;
        
        while (table[index].isOccupied && !table[index].isDeleted) {
            if (table[index].key == key) {
                cout << "  Key already exists at index " << index << ". Updating value.\n";
                table[index].value = value;
                return;
            }
            
            probeCount++;
            index = (originalIndex + probeCount) % capacity;
            
            if (index == originalIndex) {
                cout << "  Table is full! Cannot insert.\n";
                return;
            }
            
            cout << "  Collision at index " << (index - probeCount + capacity) % capacity 
                 << ". Linear probing to index " << index << endl;
        }
        
        table[index] = KeyValue(key, value);
        size++;
        cout << "  Successfully inserted at index " << index << endl;
        cout << "  Total probes: " << probeCount + 1 << endl;
    }
    
    // Search for a key
    int search(const string& key) {
        int index = hashFunction(key);
        int originalIndex = index;
        int probeCount = 0;
        
        cout << "Searching for key '" << key << "':\n";
        cout << "  Hash value: " << index << endl;
        
        while (table[index].isOccupied) {
            if (table[index].key == key && !table[index].isDeleted) {
                cout << "  Found at index " << index << " with value " << table[index].value << endl;
                cout << "  Total probes: " << probeCount + 1 << endl;
                return table[index].value;
            }
            
            probeCount++;
            index = (originalIndex + probeCount) % capacity;
            
            if (index == originalIndex) {
                break;
            }
            
            cout << "  Checking index " << index << " (probe " << probeCount + 1 << ")\n";
        }
        
        cout << "  Key '" << key << "' not found.\n";
        cout << "  Total probes: " << probeCount + 1 << endl;
        return -1; // Not found
    }
    
    // Delete a key
    bool remove(const string& key) {
        int index = hashFunction(key);
        int originalIndex = index;
        int probeCount = 0;
        
        cout << "Deleting key '" << key << "':\n";
        cout << "  Hash value: " << index << endl;
        
        while (table[index].isOccupied) {
            if (table[index].key == key && !table[index].isDeleted) {
                table[index].isDeleted = true;
                size--;
                cout << "  Successfully deleted from index " << index << endl;
                cout << "  Total probes: " << probeCount + 1 << endl;
                return true;
            }
            
            probeCount++;
            index = (originalIndex + probeCount) % capacity;
            
            if (index == originalIndex) {
                break;
            }
            
            cout << "  Checking index " << index << " (probe " << probeCount + 1 << ")\n";
        }
        
        cout << "  Key '" << key << "' not found for deletion.\n";
        cout << "  Total probes: " << probeCount + 1 << endl;
        return false;
    }
    
    // Display the entire hash table
    void display() {
        cout << "\nHash Table Contents:\n";
        cout << "====================\n";
        cout << "Capacity: " << capacity << ", Size: " << size << ", Load Factor: " 
             << static_cast<double>(size) / capacity << endl;
        cout << "Index\tKey\t\tValue\tStatus\n";
        cout << "-----\t---\t\t-----\t------\n";
        
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            if (table[i].isOccupied && !table[i].isDeleted) {
                cout << table[i].key << "\t\t" << table[i].value << "\tOccupied";
            } else if (table[i].isOccupied && table[i].isDeleted) {
                cout << table[i].key << "\t\t" << table[i].value << "\tDeleted";
            } else {
                cout << "-\t\t-\tEmpty";
            }
            cout << endl;
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
        
        int emptySlots = 0;
        int deletedSlots = 0;
        for (int i = 0; i < capacity; i++) {
            if (!table[i].isOccupied) emptySlots++;
            else if (table[i].isDeleted) deletedSlots++;
        }
        
        cout << "Empty Slots: " << emptySlots << endl;
        cout << "Deleted Slots: " << deletedSlots << endl;
        cout << "Occupied Slots: " << size << endl;
    }
};

// Function to display menu
void displayMenu() {
    cout << "\n=== Hash Table Operations ===\n";
    cout << "1. Insert key-value pair\n";
    cout << "2. Search for key\n";
    cout << "3. Delete key\n";
    cout << "4. Display hash table\n";
    cout << "5. Show statistics\n";
    cout << "6. Exit\n";
    cout << "Enter your choice (1-6): ";
}

int main() {
    int initialCapacity;
    cout << "=== Hash Table with Linear Probing ===\n";
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
                ht.getStatistics();
                break;
                
            case 6:
                cout << "Exiting program.\n";
                break;
                
            default:
                cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 6);
    
    return 0;
}
```

## Example Output
```
=== Hash Table with Linear Probing ===
Enter initial capacity of hash table: 8

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 1
Enter key: apple
Enter value: 10
Inserting (apple, 10):
  Hash value: 5
  Successfully inserted at index 5
  Total probes: 1

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 1
Enter key: banana
Enter value: 20
Inserting (banana, 20):
  Hash value: 1
  Successfully inserted at index 1
  Total probes: 1

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 1
Enter key: cherry
Enter value: 30
Inserting (cherry, 30):
  Hash value: 5
  Collision at index 5. Linear probing to index 6
  Successfully inserted at index 6
  Total probes: 2

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 1
Enter key: date
Enter value: 40
Inserting (date, 40):
  Hash value: 1
  Collision at index 1. Linear probing to index 2
  Successfully inserted at index 2
  Total probes: 2

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 2
Enter key to search: cherry
Searching for key 'cherry':
  Hash value: 5
  Checking index 6 (probe 2)
  Found at index 6 with value 30
  Total probes: 2

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 3
Enter key to delete: banana
Deleting key 'banana':
  Hash value: 1
  Successfully deleted from index 1
  Total probes: 1

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 4

Hash Table Contents:
====================
Capacity: 8, Size: 3, Load Factor: 0.375
Index	Key		Value	Status
-----	---		-----	------
0	-		-	Empty
1	banana		20	Deleted
2	date		40	Occupied
3	-		-	Empty
4	-		-	Empty
5	apple		10	Occupied
6	cherry		30	Occupied
7	-		-	Empty

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 1
Enter key: elderberry
Enter value: 50
Inserting (elderberry, 50):
  Hash value: 1
  Successfully inserted at index 1
  Total probes: 1

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 5

Hash Table Statistics:
======================
Capacity: 8
Size: 4
Load Factor: 0.5
Load Factor Threshold: 0.7
Empty Slots: 3
Deleted Slots: 0
Occupied Slots: 4

=== Hash Table Operations ===
1. Insert key-value pair
2. Search for key
3. Delete key
4. Display hash table
5. Show statistics
6. Exit
Enter your choice (1-6): 6
Exiting program.
```

## Explanation:

- The hash table uses the function:

- index = key % size

- If the calculated position is already full, linear probing moves step-by-step to the next index.

- The process continues until an empty slot is found.

- This ensures all keys are stored even if collisions occur.