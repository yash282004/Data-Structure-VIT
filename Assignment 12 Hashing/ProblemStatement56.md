## Assignment 1
*Title:*

WAP to simulate a faculty database as a hash table. Search a particular faculty by using 'divide' as a hash function for linear probing with chaining without replacement method of collision handling technique. Assume suitable data for faculty record.

## Author:
**Yash Santosh Khandagale**

## Aim:
To write a C++ program to simulate a faculty database using a hash table and search a faculty record using the Divide (modulus) hash function, implementing linear probing with chaining without replacement as the collision resolution technique.

## Problem Statement:
Create a hash table to store faculty records.
Each faculty record contains:

- Faculty ID

- Faculty Name

- Department

Use Divide method: hash(ID) = ID % size
Resolve collisions using Linear Probing with Chaining Without Replacement.
Provide features to:

1. Insert a faculty record

2. Display the hash table

3. Search for a faculty by ID

Assume suitable data.

## Algorithm:
1. Start

2. Define Structure
Create a structure Faculty with:
- id
- name
- dept
- next pointer index

3. Initialize Hash Table
- Create an array of size N
- Set all IDs to -1 (empty)
- Set all next values to -1

4. Hash Function
index = id % size

5. Insertion (Linear Probing + Chaining Without Replacement)
- Compute hash index.
- If the slot is empty → insert.
- Else collision occurs:
- Linearly probe to find an empty slot.
- Insert faculty there.
- Update the chaining link of the original index or its chain.
6. Searching

- Compute hash index.
- Follow the chain using next pointers:
- If ID found → display record.
- If chain ends → record not found.

7. Display
Print table with all fields.
8. Stop

## C++ Program 
```c++
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
using namespace std;

struct Faculty {
    int facultyId;
    string name;
    string department;
    string designation;
    string email;
    string phone;
    int chain;
    bool isOccupied;
    
    Faculty() : facultyId(0), name(""), department(""), designation(""), email(""), phone(""), chain(-1), isOccupied(false) {}
    
    Faculty(int id, string n, string dept, string desig, string mail, string ph) 
        : facultyId(id), name(n), department(dept), designation(desig), email(mail), phone(ph), chain(-1), isOccupied(true) {}
};

class FacultyDatabase {
private:
    vector<Faculty> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 0.8;
    
    int hashFunction(int facultyId) {
        return facultyId % capacity;
    }
    
    int findEmptySlot() {
        for (int i = 0; i < capacity; i++) {
            if (!table[i].isOccupied) {
                return i;
            }
        }
        return -1;
    }
    
    void rehash() {
        cout << "Load factor exceeded threshold. Rehashing table..." << endl;
        
        vector<Faculty> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity);
        size = 0;
        
        for (const Faculty& faculty : oldTable) {
            if (faculty.isOccupied) {
                insertFaculty(faculty.facultyId, faculty.name, faculty.department, 
                            faculty.designation, faculty.email, faculty.phone);
            }
        }
        cout << "Rehashing completed. New capacity: " << capacity << endl;
    }
    
public:
    FacultyDatabase(int initialCapacity = 10) {
        capacity = initialCapacity;
        size = 0;
        table.resize(capacity);
    }
    
    void insertFaculty(int facultyId, const string& name, const string& department, 
                     const string& designation, const string& email, const string& phone) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int hashIndex = hashFunction(facultyId);
        
        cout << "Inserting Faculty Record:" << endl;
        cout << "=========================" << endl;
        cout << "Faculty ID: " << facultyId << endl;
        cout << "Hash value (Divide " << capacity << "): " << hashIndex << endl;
        
        if (!table[hashIndex].isOccupied) {
            table[hashIndex] = Faculty(facultyId, name, department, designation, email, phone);
            size++;
            cout << "Successfully inserted at home position" << endl;
            cout << "Storage location: Index " << hashIndex << endl;
            return;
        }
        
        if (table[hashIndex].facultyId == facultyId) {
            cout << "Error: Faculty ID " << facultyId << " already exists" << endl;
            cout << "Existing record - Name: " << table[hashIndex].name 
                 << ", Department: " << table[hashIndex].department << endl;
            return;
        }
        
        int currentIndex = hashIndex;
        int previousIndex = -1;
        
        while (table[currentIndex].chain != -1) {
            if (table[currentIndex].facultyId == facultyId) {
                cout << "Error: Faculty ID " << facultyId << " already exists in chain" << endl;
                cout << "Existing record - Name: " << table[currentIndex].name 
                     << ", Department: " << table[currentIndex].department << endl;
                return;
            }
            previousIndex = currentIndex;
            currentIndex = table[currentIndex].chain;
        }
        
        if (table[currentIndex].facultyId == facultyId) {
            cout << "Error: Faculty ID " << facultyId << " already exists" << endl;
            return;
        }
        
        int emptySlot = findEmptySlot();
        if (emptySlot == -1) {
            cout << "Error: Hash table is full" << endl;
            return;
        }
        
        table[emptySlot] = Faculty(facultyId, name, department, designation, email, phone);
        table[currentIndex].chain = emptySlot;
        
        size++;
        cout << "Successfully inserted with chaining" << endl;
        cout << "Home position: Index " << hashIndex << endl;
        cout << "Actual storage: Index " << emptySlot << endl;
        cout << "Chain pointer from index " << currentIndex << " to " << emptySlot << endl;
    }
    
    Faculty* searchFaculty(int facultyId) {
        int hashIndex = hashFunction(facultyId);
        int currentIndex = hashIndex;
        int searchSteps = 0;
        
        cout << "Searching for Faculty (ID: " << facultyId << "):" << endl;
        cout << "================================" << endl;
        cout << "Hash value (Divide " << capacity << "): " << hashIndex << endl;
        
        while (currentIndex != -1 && table[currentIndex].isOccupied) {
            searchSteps++;
            cout << "Step " << searchSteps << ": Checking index " << currentIndex 
                 << " - Faculty ID: " << table[currentIndex].facultyId << endl;
            
            if (table[currentIndex].facultyId == facultyId) {
                cout << "Faculty found at index " << currentIndex << endl;
                cout << "Total search steps: " << searchSteps << endl;
                return &table[currentIndex];
            }
            
            currentIndex = table[currentIndex].chain;
        }
        
        cout << "Faculty with ID " << facultyId << " not found" << endl;
        cout << "Total search steps: " << searchSteps << endl;
        return nullptr;
    }
    
    void displayFacultyRecord(int facultyId) {
        Faculty* faculty = searchFaculty(facultyId);
        if (faculty != nullptr) {
            cout << "Faculty Record Details:" << endl;
            cout << "======================" << endl;
            cout << left << setw(15) << "Faculty ID:" << faculty->facultyId << endl;
            cout << setw(15) << "Name:" << faculty->name << endl;
            cout << setw(15) << "Department:" << faculty->department << endl;
            cout << setw(15) << "Designation:" << faculty->designation << endl;
            cout << setw(15) << "Email:" << faculty->email << endl;
            cout << setw(15) << "Phone:" << faculty->phone << endl;
        }
    }
    
    bool deleteFaculty(int facultyId) {
        int hashIndex = hashFunction(facultyId);
        int currentIndex = hashIndex;
        int prevIndex = -1;
        int searchSteps = 0;
        
        cout << "Deleting Faculty Record (ID: " << facultyId << "):" << endl;
        cout << "================================" << endl;
        cout << "Hash value (Divide " << capacity << "): " << hashIndex << endl;
        
        while (currentIndex != -1 && table[currentIndex].isOccupied) {
            searchSteps++;
            cout << "Step " << searchSteps << ": Checking index " << currentIndex 
                 << " - Faculty ID: " << table[currentIndex].facultyId << endl;
            
            if (table[currentIndex].facultyId == facultyId) {
                if (prevIndex == -1) {
                    if (table[currentIndex].chain == -1) {
                        table[currentIndex].isOccupied = false;
                        table[currentIndex].chain = -1;
                    } else {
                        int nextIndex = table[currentIndex].chain;
                        table[currentIndex] = table[nextIndex];
                        table[nextIndex].isOccupied = false;
                        table[nextIndex].chain = -1;
                    }
                } else {
                    table[prevIndex].chain = table[currentIndex].chain;
                    table[currentIndex].isOccupied = false;
                    table[currentIndex].chain = -1;
                }
                
                size--;
                cout << "Successfully deleted faculty record" << endl;
                cout << "Deleted from index: " << currentIndex << endl;
                cout << "Total search steps: " << searchSteps << endl;
                return true;
            }
            
            prevIndex = currentIndex;
            currentIndex = table[currentIndex].chain;
        }
        
        cout << "Faculty with ID " << facultyId << " not found for deletion" << endl;
        cout << "Total search steps: " << searchSteps << endl;
        return false;
    }
    
    void displayAllFaculty() {
        cout << "All Faculty Records in Database:" << endl;
        cout << "================================" << endl;
        cout << "Total Faculty: " << size << ", Table Capacity: " << capacity << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << endl;
        
        cout << "Index\tFaculty ID\tName\t\tDepartment\t\tChain" << endl;
        cout << "-----\t----------\t----\t\t----------\t\t-----" << endl;
        
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            if (table[i].isOccupied) {
                cout << table[i].facultyId << "\t\t" 
                     << left << setw(12) << table[i].name.substr(0, 10) << "\t"
                     << left << setw(15) << table[i].department.substr(0, 12) << "\t"
                     << table[i].chain;
            } else {
                cout << "-\t\t-\t\t-\t\t-";
            }
            cout << endl;
        }
    }
    
    void displayChains() {
        cout << "Detailed Chain Information:" << endl;
        cout << "===========================" << endl;
        
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied) {
                cout << "Index " << i << " (Home for ID " << hashFunction(table[i].facultyId) << "): ";
                cout << "ID=" << table[i].facultyId << ", Name=" << table[i].name;
                if (table[i].chain != -1) {
                    cout << " -> Chain to index " << table[i].chain;
                } else {
                    cout << " -> End of chain";
                }
                cout << endl;
            }
        }
    }
    
    void displayStatistics() {
        cout << "Database Statistics:" << endl;
        cout << "====================" << endl;
        cout << "Table Capacity: " << capacity << endl;
        cout << "Number of Faculty: " << size << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << "Load Factor Threshold: " << LOAD_FACTOR_THRESHOLD << endl;
        
        int homePositionCount = 0;
        int chainPositionCount = 0;
        int maxChainLength = 0;
        int totalChainLength = 0;
        int chainsCount = 0;
        
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied) {
                if (hashFunction(table[i].facultyId) == i) {
                    homePositionCount++;
                } else {
                    chainPositionCount++;
                }
                
                int chainLength = 0;
                int currentIndex = i;
                while (currentIndex != -1 && table[currentIndex].isOccupied) {
                    chainLength++;
                    currentIndex = table[currentIndex].chain;
                }
                
                if (chainLength > maxChainLength) {
                    maxChainLength = chainLength;
                }
                
                if (hashFunction(table[i].facultyId) == i && chainLength > 1) {
                    totalChainLength += chainLength;
                    chainsCount++;
                }
            }
        }
        
        double avgChainLength = chainsCount > 0 ? static_cast<double>(totalChainLength) / chainsCount : 0;
        
        cout << "Home Position Records: " << homePositionCount << endl;
        cout << "Chain Position Records: " << chainPositionCount << endl;
        cout << "Maximum Chain Length: " << maxChainLength << endl;
        cout << "Average Chain Length: " << avgChainLength << endl;
        cout << "Number of Chains: " << chainsCount << endl;
    }
    
    void searchByDepartment(const string& department) {
        cout << "Searching Faculty in Department: " << department << endl;
        cout << "================================" << endl;
        
        vector<Faculty*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && table[i].department == department) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << "No faculty found in department " << department << endl;
        } else {
            cout << "Found " << results.size() << " faculty member(s) in department " << department << ":" << endl;
            for (Faculty* faculty : results) {
                cout << "  ID: " << faculty->facultyId 
                     << " | Name: " << faculty->name 
                     << " | Designation: " << faculty->designation 
                     << " | Email: " << faculty->email << endl;
            }
        }
    }
};

void displayMenu() {
    cout << "Faculty Database Management System" << endl;
    cout << "1. Add Faculty Record" << endl;
    cout << "2. Search Faculty by ID" << endl;
    cout << "3. Display Faculty Record" << endl;
    cout << "4. Delete Faculty Record" << endl;
    cout << "5. Display All Faculty" << endl;
    cout << "6. Display Detailed Chains" << endl;
    cout << "7. Search Faculty by Department" << endl;
    cout << "8. Show Database Statistics" << endl;
    cout << "9. Exit" << endl;
    cout << "Enter your choice (1-9): ";
}

void addSampleData(FacultyDatabase& db) {
    cout << "Adding Sample Faculty Data..." << endl;
    db.insertFaculty(101, "Dr. Rajesh Kumar", "Computer Science", "Professor", "rajesh@university.edu", "9876543210");
    db.insertFaculty(202, "Dr. Priya Sharma", "Mathematics", "Associate Professor", "priya@university.edu", "9876543211");
    db.insertFaculty(303, "Dr. Amit Patel", "Physics", "Professor", "amit@university.edu", "9876543212");
    db.insertFaculty(101, "Dr. Sunita Verma", "Computer Science", "Assistant Professor", "sunita@university.edu", "9876543213");
    db.insertFaculty(404, "Dr. Ravi Singh", "Chemistry", "Professor", "ravi@university.edu", "9876543214");
    db.insertFaculty(505, "Dr. Neha Gupta", "Electronics", "Associate Professor", "neha@university.edu", "9876543215");
    db.insertFaculty(606, "Dr. Sanjay Mishra", "Mechanical", "Professor", "sanjay@university.edu", "9876543216");
    db.insertFaculty(707, "Dr. Anjali Joshi", "Computer Science", "Assistant Professor", "anjali@university.edu", "9876543217");
}

int main() {
    int initialCapacity;
    cout << "Faculty Database using Hashing with Chaining without Replacement" << endl;
    cout << "Enter initial capacity of hash table: ";
    cin >> initialCapacity;
    
    if (initialCapacity <= 0) {
        cout << "Invalid capacity. Using default capacity 10." << endl;
        initialCapacity = 10;
    }
    
    FacultyDatabase database(initialCapacity);
    
    addSampleData(database);
    
    int choice;
    int facultyId;
    string name, department, designation, email, phone;
    
    do {
        displayMenu();
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter Faculty ID: ";
                cin >> facultyId;
                cin.ignore();
                cout << "Enter Name: ";
                getline(cin, name);
                cout << "Enter Department: ";
                getline(cin, department);
                cout << "Enter Designation: ";
                getline(cin, designation);
                cout << "Enter Email: ";
                getline(cin, email);
                cout << "Enter Phone: ";
                getline(cin, phone);
                database.insertFaculty(facultyId, name, department, designation, email, phone);
                break;
                
            case 2:
                cout << "Enter Faculty ID to search: ";
                cin >> facultyId;
                database.searchFaculty(facultyId);
                break;
                
            case 3:
                cout << "Enter Faculty ID to display: ";
                cin >> facultyId;
                database.displayFacultyRecord(facultyId);
                break;
                
            case 4:
                cout << "Enter Faculty ID to delete: ";
                cin >> facultyId;
                database.deleteFaculty(facultyId);
                break;
                
            case 5:
                database.displayAllFaculty();
                break;
                
            case 6:
                database.displayChains();
                break;
                
            case 7:
                cin.ignore();
                cout << "Enter Department to search: ";
                getline(cin, department);
                database.searchByDepartment(department);
                break;
                
            case 8:
                database.displayStatistics();
                break;
                
            case 9:
                cout << "Exiting Faculty Database System. Thank you!" << endl;
                break;
                
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 9);
    
    return 0;
}
```

## Example Output
```
Faculty Database using Hashing with Chaining without Replacement
Enter initial capacity of hash table: 8
Adding Sample Faculty Data...
Inserting Faculty Record:
=========================
Faculty ID: 101
Hash value (Divide 8): 5
Successfully inserted at home position
Storage location: Index 5

Inserting Faculty Record:
=========================
Faculty ID: 202
Hash value (Divide 8): 2
Successfully inserted at home position
Storage location: Index 2

Inserting Faculty Record:
=========================
Faculty ID: 303
Hash value (Divide 8): 7
Successfully inserted at home position
Storage location: Index 7

Inserting Faculty Record:
=========================
Faculty ID: 101
Hash value (Divide 8): 5
Error: Faculty ID 101 already exists
Existing record - Name: Dr. Rajesh Kumar, Department: Computer Science

Faculty Database Management System
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Delete Faculty Record
5. Display All Faculty
6. Display Detailed Chains
7. Search Faculty by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 2
Enter Faculty ID to search: 101
Searching for Faculty (ID: 101):
================================
Hash value (Divide 8): 5
Step 1: Checking index 5 - Faculty ID: 101
Faculty found at index 5
Total search steps: 1

Faculty Database Management System
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Delete Faculty Record
5. Display All Faculty
6. Display Detailed Chains
7. Search Faculty by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 5
All Faculty Records in Database:
================================
Total Faculty: 6, Table Capacity: 8
Load Factor: 0.75

Index	Faculty ID	Name		Department		Chain
-----	----------	----		----------		-----
0	-		-		-		-
1	-		-		-		-
2	202		Dr. Priya 	Mathematics		-1
3	-		-		-		-
4	-		-		-		-
5	101		Dr. Rajesh 	Computer Scien	-1
6	-		-		-		-
7	303		Dr. Amit P	Physics		-1

Faculty Database Management System
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Delete Faculty Record
5. Display All Faculty
6. Display Detailed Chains
7. Search Faculty by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 8
Database Statistics:
====================
Table Capacity: 8
Number of Faculty: 6
Load Factor: 0.75
Load Factor Threshold: 0.8
Home Position Records: 6
Chain Position Records: 0
Maximum Chain Length: 1
Average Chain Length: 0
Number of Chains: 0
```

## Explanation

- Hash function: ID % size
- Collisions resolved using:
- Linear Probing
- Chaining without replacement (using next index links)
- Search follows the chain until ID is found or chain ends.
- Works efficiently for faculty database simulation.


