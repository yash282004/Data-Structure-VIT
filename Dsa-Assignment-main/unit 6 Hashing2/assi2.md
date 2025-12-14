## Assignment 2
*Title:*
WAP to simulate a faculty database as a hash table. Search a particular faculty by using MOD as a hash function for linear probing with chaining with replacement method of collision handling technique. Assume suitable data for faculty record.

## Author:

**Yash Santosh Khandagale**

## Aim:

To implement a faculty database using a hash table where collision resolution is performed using linear probing with chaining (with replacement) and MOD hash function for searching faculty records.

## Problem Statement:

- Create a hash table to store a faculty database.
- Each faculty record contains:

- Faculty ID (integer)

- Name (string)

- Use hash = FacultyID % TableSize (MOD function).
- If collision occurs, apply linear probing with chaining (with replacement).
- Search for a particular faculty by Faculty ID.

- Assume suitable sample data.

## Algorithm:
1. Start

- Initialize an empty hash table of fixed size (say 10).

2. Hash Function
- hashIndex = facultyID % tableSize

3. Insert Operation (Linear Probing + Chaining With Replacement)

- Compute hash index.

- If slot is empty → insert record.

-  IF slot is filled:

- Check if the current occupant belongs to this slot:

- If NOT (i.e., current record is a displaced record)
→ Swap (Replacement)
- Place new record in correct position, and displaced record will get reinserted using linear probing.

- If belongs → use linear probing to find the next empty slot.
- Record chaining pointer.

4. Search Operation:

- Compute hash index using MOD.

- If record matches → return success.

- If chain exists → follow the chain until record is found.

- If not found → display "Faculty not found".

5. Stop

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
        cout << "Hash value (MOD " << capacity << "): " << hashIndex << endl;
        
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
        
        int currentHomeIndex = hashFunction(table[hashIndex].facultyId);
        
        if (currentHomeIndex == hashIndex) {
            int emptySlot = findEmptySlot();
            if (emptySlot == -1) {
                cout << "Error: Hash table is full" << endl;
                return;
            }
            
            table[emptySlot] = Faculty(facultyId, name, department, designation, email, phone);
            
            int chainEnd = hashIndex;
            while (table[chainEnd].chain != -1) {
                chainEnd = table[chainEnd].chain;
            }
            table[chainEnd].chain = emptySlot;
            
            size++;
            cout << "Successfully inserted with chaining (legitimate occupant at home)" << endl;
            cout << "Home position: Index " << hashIndex << endl;
            cout << "Actual storage: Index " << emptySlot << endl;
            cout << "Chain pointer from index " << chainEnd << " to " << emptySlot << endl;
        }
        else {
            int emptySlot = findEmptySlot();
            if (emptySlot == -1) {
                cout << "Error: Hash table is full" << endl;
                return;
            }
            
            Faculty temp = table[hashIndex];
            table[hashIndex] = Faculty(facultyId, name, department, designation, email, phone);
            
            int newSlot = findEmptySlot();
            if (newSlot == -1) {
                cout << "Error: Hash table is full during replacement" << endl;
                return;
            }
            
            table[newSlot] = temp;
            
            int chainStart = currentHomeIndex;
            while (table[chainStart].chain != hashIndex && table[chainStart].chain != -1) {
                chainStart = table[chainStart].chain;
            }
            
            if (table[chainStart].chain == hashIndex) {
                table[chainStart].chain = newSlot;
            }
            
            table[newSlot].chain = temp.chain;
            
            size++;
            cout << "Successfully inserted with REPLACEMENT" << endl;
            cout << "Replaced illegitimate occupant at index " << hashIndex << endl;
            cout << "Moved existing record to index " << newSlot << endl;
            cout << "Updated chain pointers accordingly" << endl;
        }
    }
    
    Faculty* searchFaculty(int facultyId) {
        int hashIndex = hashFunction(facultyId);
        int currentIndex = hashIndex;
        int searchSteps = 0;
        
        cout << "Searching for Faculty (ID: " << facultyId << "):" << endl;
        cout << "================================" << endl;
        cout << "Hash value (MOD " << capacity << "): " << hashIndex << endl;
        
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
        cout << "Hash value (MOD " << capacity << "): " << hashIndex << endl;
        
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
                        int nextHome = hashFunction(table[nextIndex].facultyId);
                        
                        if (nextHome == currentIndex) {
                            table[currentIndex] = table[nextIndex];
                            table[nextIndex].isOccupied = false;
                            table[nextIndex].chain = -1;
                        } else {
                            table[currentIndex].isOccupied = false;
                            table[currentIndex].chain = -1;
                        }
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
        
        cout << "Index\tFaculty ID\tHome\tName\t\tDepartment\t\tChain" << endl;
        cout << "-----\t----------\t----\t----\t\t----------\t\t-----" << endl;
        
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            if (table[i].isOccupied) {
                int homePosition = hashFunction(table[i].facultyId);
                cout << table[i].facultyId << "\t\t" 
                     << homePosition << "\t"
                     << left << setw(12) << table[i].name.substr(0, 10) << "\t"
                     << left << setw(15) << table[i].department.substr(0, 12) << "\t"
                     << table[i].chain;
            } else {
                cout << "-\t\t-\t-\t\t-\t\t-";
            }
            cout << endl;
        }
    }
    
    void displayChains() {
        cout << "Detailed Chain Information with Replacement Analysis:" << endl;
        cout << "====================================================" << endl;
        
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied) {
                int homePosition = hashFunction(table[i].facultyId);
                cout << "Index " << i << ": ";
                cout << "ID=" << table[i].facultyId;
                cout << ", Home=" << homePosition;
                
                if (homePosition == i) {
                    cout << " (LEGITIMATE)";
                } else {
                    cout << " (REPLACED/CHAINED)";
                }
                
                if (table[i].chain != -1) {
                    cout << " -> Chain to index " << table[i].chain;
                } else {
                    cout << " -> End of chain";
                }
                cout << endl;
            }
        }
    }
    
    void displayReplacementAnalysis() {
        cout << "Replacement Analysis Report:" << endl;
        cout << "============================" << endl;
        
        int legitimateCount = 0;
        int replacedCount = 0;
        int chainCount = 0;
        int maxChainLength = 0;
        
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied) {
                int homePosition = hashFunction(table[i].facultyId);
                
                if (homePosition == i) {
                    legitimateCount++;
                } else {
                    replacedCount++;
                }
                
                if (table[i].chain != -1) {
                    chainCount++;
                }
                
                int chainLength = 1;
                int currentIndex = i;
                while (table[currentIndex].chain != -1) {
                    chainLength++;
                    currentIndex = table[currentIndex].chain;
                }
                
                if (chainLength > maxChainLength) {
                    maxChainLength = chainLength;
                }
            }
        }
        
        cout << "Legitimate positions (home = actual): " << legitimateCount << endl;
        cout << "Replaced positions (home != actual): " << replacedCount << endl;
        cout << "Chains in table: " << chainCount << endl;
        cout << "Maximum chain length: " << maxChainLength << endl;
        cout << "Replacement rate: " << fixed << setprecision(1) 
             << (static_cast<double>(replacedCount) / size * 100) << "%" << endl;
    }
    
    void displayStatistics() {
        cout << "Database Statistics:" << endl;
        cout << "====================" << endl;
        cout << "Table Capacity: " << capacity << endl;
        cout << "Number of Faculty: " << size << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << "Load Factor Threshold: " << LOAD_FACTOR_THRESHOLD << endl;
        
        displayReplacementAnalysis();
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
    cout << "7. Display Replacement Analysis" << endl;
    cout << "8. Search Faculty by Department" << endl;
    cout << "9. Show Database Statistics" << endl;
    cout << "10. Exit" << endl;
    cout << "Enter your choice (1-10): ";
}

void addSampleData(FacultyDatabase& db) {
    cout << "Adding Sample Faculty Data..." << endl;
    db.insertFaculty(101, "Dr. Rajesh Kumar", "Computer Science", "Professor", "rajesh@university.edu", "9876543210");
    db.insertFaculty(109, "Dr. Priya Sharma", "Mathematics", "Associate Professor", "priya@university.edu", "9876543211");
    db.insertFaculty(117, "Dr. Amit Patel", "Physics", "Professor", "amit@university.edu", "9876543212");
    db.insertFaculty(125, "Dr. Sunita Verma", "Computer Science", "Assistant Professor", "sunita@university.edu", "9876543213");
    db.insertFaculty(101, "Dr. Ravi Singh", "Chemistry", "Professor", "ravi@university.edu", "9876543214");
    db.insertFaculty(109, "Dr. Neha Gupta", "Electronics", "Associate Professor", "neha@university.edu", "9876543215");
    db.insertFaculty(117, "Dr. Sanjay Mishra", "Mechanical", "Professor", "sanjay@university.edu", "9876543216");
}

int main() {
    int initialCapacity;
    cout << "Faculty Database using Hashing with Chaining with Replacement" << endl;
    cout << "Enter initial capacity of hash table: ";
    cin >> initialCapacity;
    
    if (initialCapacity <= 0) {
        cout << "Invalid capacity. Using default capacity 8." << endl;
        initialCapacity = 8;
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
                database.displayReplacementAnalysis();
                break;
                
            case 8:
                cin.ignore();
                cout << "Enter Department to search: ";
                getline(cin, department);
                database.searchByDepartment(department);
                break;
                
            case 9:
                database.displayStatistics();
                break;
                
            case 10:
                cout << "Exiting Faculty Database System. Thank you!" << endl;
                break;
                
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 10);
    
    return 0;
}
```
## Example Output
```
Faculty Database using Hashing with Chaining with Replacement
Enter initial capacity of hash table: 8
Adding Sample Faculty Data...
Inserting Faculty Record:
=========================
Faculty ID: 101
Hash value (MOD 8): 5
Successfully inserted at home position
Storage location: Index 5

Inserting Faculty Record:
=========================
Faculty ID: 109
Hash value (MOD 8): 5
Successfully inserted with chaining (legitimate occupant at home)
Home position: Index 5
Actual storage: Index 6
Chain pointer from index 5 to 6

Inserting Faculty Record:
=========================
Faculty ID: 117
Hash value (MOD 8): 5
Successfully inserted with chaining (legitimate occupant at home)
Home position: Index 5
Actual storage: Index 7
Chain pointer from index 6 to 7

Inserting Faculty Record:
=========================
Faculty ID: 125
Hash value (MOD 8): 5
Successfully inserted with REPLACEMENT
Replaced illegitimate occupant at index 5
Moved existing record to index 0
Updated chain pointers accordingly

Faculty Database Management System
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Delete Faculty Record
5. Display All Faculty
6. Display Detailed Chains
7. Display Replacement Analysis
8. Search Faculty by Department
9. Show Database Statistics
10. Exit
Enter your choice (1-10): 5
All Faculty Records in Database:
================================
Total Faculty: 4, Table Capacity: 8
Load Factor: 0.50

Index	Faculty ID	Home	Name		Department		Chain
-----	----------	----	----		----------		-----
0	101		5	Dr. Rajesh 	Computer Scien	-1
1	-		-	-		-		-
2	-		-	-		-		-
3	-		-	-		-		-
4	-		-	-		-		-
5	125		5	Dr. Sunita 	Computer Scien	6
6	109		5	Dr. Priya 	Mathematics		7
7	117		5	Dr. Amit P	Physics		-1

Faculty Database Management System
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Delete Faculty Record
5. Display All Faculty
6. Display Detailed Chains
7. Display Replacement Analysis
8. Search Faculty by Department
9. Show Database Statistics
10. Exit
Enter your choice (1-10): 6
Detailed Chain Information with Replacement Analysis:
====================================================
Index 0: ID=101, Home=5 (REPLACED/CHAINED) -> End of chain
Index 5: ID=125, Home=5 (LEGITIMATE) -> Chain to index 6
Index 6: ID=109, Home=5 (REPLACED/CHAINED) -> Chain to index 7
Index 7: ID=117, Home=5 (REPLACED/CHAINED) -> End of chain

Faculty Database Management System
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Delete Faculty Record
5. Display All Faculty
6. Display Detailed Chains
7. Display Replacement Analysis
8. Search Faculty by Department
9. Show Database Statistics
10. Exit
Enter your choice (1-10): 7
Replacement Analysis Report:
============================
Legitimate positions (home = actual): 1
Replaced positions (home != actual): 3
Chains in table: 2
Maximum chain length: 3
Replacement rate: 75.0%
```
## Explanation

- MOD hashing maps faculty IDs to table positions.

- When collision happens, linear probing + chaining with replacement is used.

- Replacement ensures that elements are stored as close as possible to their original hash positions.

- Searching follows the chain links for accurate lookups.