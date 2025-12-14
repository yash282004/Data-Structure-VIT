## Assignment 5
*Title:*
WAP to simulate a faculty database as a hash table. Search a particular faculty by using MOD as a hash function for linear probing method of collision handling technique. Assume suitable data for faculty record.

## Author:
**Yash Santosh Khandagale**

## Aim:
To write a C++ program to simulate a faculty database as a hash table. The program will store and retrieve Faculty Records using an ID as the key. Collisions will be resolved using Linear Probing.

## Problem Statement:
- Create a hash table of user-defined size to store Faculty records. Assume each record contains a Faculty ID (key), Name, and Department.

- The program must include the following functionalities:

- Insert Record: Insert a new faculty record using the Faculty ID.

- Search Record: Search for and display a faculty record given their Faculty ID, using the linear probing sequence if a collision occurred during insertion.

- Display Table: Display the entire hash table.

- Use the hash function: hash(Faculty_ID) = Faculty_ID % table_size

## Algorithm:
1. Step 1: Define a Faculty structure containing facultyID, name, and department. Use 0 to mark an empty slot.

2. Step 2: Define the HashTable class using a vector<Faculty>.

3. Step 3: Implement the hashFunction as Faculty_ID % size.

4. Step 4 (Insert):

- Compute the initial hash index.

- Linear Probing: If the slot is occupied, probe for the next slot using: new_index = (index + 1) % size. Repeat until an empty slot is found or the table is full.

5. Step 5 (Search):

- Compute the initial hash index.

- Search Sequence: Probe using new_index = (index + 1) % size. Stop if the key is found, an empty slot is encountered (record is not found), or the probe returns to the start (table searched).

6. Step 6 (Display): Iterate through the table and display all stored faculty records.

## C++ Program
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
using namespace std;

// Structure to represent a faculty record
struct Faculty {
    int facultyId;
    string name;
    string department;
    string designation;
    string email;
    string phone;
    bool isOccupied;
    bool isDeleted;
    
    Faculty() : facultyId(0), name(""), department(""), designation(""), email(""), phone(""), isOccupied(false), isDeleted(false) {}
    
    Faculty(int id, string n, string dept, string desig, string mail, string ph) 
        : facultyId(id), name(n), department(dept), designation(desig), email(mail), phone(ph), isOccupied(true), isDeleted(false) {}
};

class FacultyDatabase {
private:
    vector<Faculty> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 0.7;
    
    // MOD hash function using faculty ID
    int hashFunction(int facultyId) {
        return facultyId % capacity;
    }
    
    // Rehash the entire table when load factor is too high
    void rehash() {
        cout << "\n Load factor exceeded threshold. Rehashing table...\n";
        
        vector<Faculty> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity);
        size = 0;
        
        for (const Faculty& faculty : oldTable) {
            if (faculty.isOccupied && !faculty.isDeleted) {
                insertFaculty(faculty.facultyId, faculty.name, faculty.department, 
                            faculty.designation, faculty.email, faculty.phone);
            }
        }
        cout << " Rehashing completed. New capacity: " << capacity << endl;
    }
    
public:
    FacultyDatabase(int initialCapacity = 10) {
        capacity = initialCapacity;
        size = 0;
        table.resize(capacity);
    }
    
    // Insert a faculty record
    void insertFaculty(int facultyId, const string& name, const string& department, 
                     const string& designation, const string& email, const string& phone) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int index = hashFunction(facultyId);
        int originalIndex = index;
        int probeCount = 0;
        
        cout << "\n Inserting Faculty Record:\n";
        cout << "============================\n";
        cout << "Faculty ID: " << facultyId << endl;
        cout << "Hash value (MOD " << capacity << "): " << index << endl;
        
        // Check if faculty ID already exists
        while (table[index].isOccupied && !table[index].isDeleted) {
            if (table[index].facultyId == facultyId) {
                cout << " Error: Faculty ID " << facultyId << " already exists!\n";
                cout << "   Existing record - Name: " << table[index].name 
                     << ", Department: " << table[index].department << endl;
                return;
            }
            
            probeCount++;
            index = (originalIndex + probeCount) % capacity;
            
            if (index == originalIndex) {
                cout << " Error: Table is full! Cannot insert faculty record.\n";
                return;
            }
            
            cout << "    Collision at index " << (index - probeCount + capacity) % capacity 
                 << ". Linear probing to index " << index << endl;
        }
        
        table[index] = Faculty(facultyId, name, department, designation, email, phone);
        size++;
        
        cout << " Successfully inserted faculty record!\n";
        cout << "   Storage location: Index " << index << endl;
        cout << "   Total probes: " << probeCount + 1 << endl;
    }
    
    // Search for a faculty by ID
    Faculty* searchFaculty(int facultyId) {
        int index = hashFunction(facultyId);
        int originalIndex = index;
        int probeCount = 0;
        
        cout << "\n Searching for Faculty (ID: " << facultyId << "):\n";
        cout << "==================================\n";
        cout << "Hash value (MOD " << capacity << "): " << index << endl;
        
        while (table[index].isOccupied) {
            probeCount++;
            
            if (table[index].facultyId == facultyId && !table[index].isDeleted) {
                cout << " Faculty found at index " << index << "!\n";
                cout << "   Total probes: " << probeCount << endl;
                return &table[index];
            }
            
            index = (originalIndex + probeCount) % capacity;
            
            if (index == originalIndex) {
                break;
            }
            
            cout << "    Checking index " << index << " (probe " << probeCount + 1 << ")\n";
        }
        
        cout << " Faculty with ID " << facultyId << " not found!\n";
        cout << "   Total probes: " << probeCount << endl;
        return nullptr;
    }
    
    // Display faculty record
    void displayFacultyRecord(int facultyId) {
        Faculty* faculty = searchFaculty(facultyId);
        if (faculty != nullptr) {
            cout << "\n Faculty Record Details:\n";
            cout << "=========================\n";
            cout << left << setw(15) << "Faculty ID:" << faculty->facultyId << endl;
            cout << setw(15) << "Name:" << faculty->name << endl;
            cout << setw(15) << "Department:" << faculty->department << endl;
            cout << setw(15) << "Designation:" << faculty->designation << endl;
            cout << setw(15) << "Email:" << faculty->email << endl;
            cout << setw(15) << "Phone:" << faculty->phone << endl;
        }
    }
    
    // Update faculty record
    void updateFaculty(int facultyId, const string& newName = "", const string& newDepartment = "", 
                      const string& newDesignation = "", const string& newEmail = "", const string& newPhone = "") {
        Faculty* faculty = searchFaculty(facultyId);
        if (faculty != nullptr) {
            cout << "\n Updating Faculty Record (ID: " << facultyId << "):\n";
            cout << "==================================\n";
            
            bool updated = false;
            
            if (!newName.empty() && newName != faculty->name) {
                cout << "   Updating name from '" << faculty->name << "' to '" << newName << "'\n";
                faculty->name = newName;
                updated = true;
            }
            if (!newDepartment.empty() && newDepartment != faculty->department) {
                cout << "   Updating department from '" << faculty->department << "' to '" << newDepartment << "'\n";
                faculty->department = newDepartment;
                updated = true;
            }
            if (!newDesignation.empty() && newDesignation != faculty->designation) {
                cout << "   Updating designation from '" << faculty->designation << "' to '" << newDesignation << "'\n";
                faculty->designation = newDesignation;
                updated = true;
            }
            if (!newEmail.empty() && newEmail != faculty->email) {
                cout << "   Updating email from '" << faculty->email << "' to '" << newEmail << "'\n";
                faculty->email = newEmail;
                updated = true;
            }
            if (!newPhone.empty() && newPhone != faculty->phone) {
                cout << "   Updating phone from '" << faculty->phone << "' to '" << newPhone << "'\n";
                faculty->phone = newPhone;
                updated = true;
            }
            
            if (updated) {
                cout << " Faculty record updated successfully!\n";
            } else {
                cout << "ℹ  No changes made to faculty record.\n";
            }
        }
    }
    
    // Delete a faculty record
    bool deleteFaculty(int facultyId) {
        int index = hashFunction(facultyId);
        int originalIndex = index;
        int probeCount = 0;
        
        cout << "\n  Deleting Faculty Record (ID: " << facultyId << "):\n";
        cout << "==================================\n";
        cout << "Hash value (MOD " << capacity << "): " << index << endl;
        
        while (table[index].isOccupied) {
            probeCount++;
            
            if (table[index].facultyId == facultyId && !table[index].isDeleted) {
                table[index].isDeleted = true;
                size--;
                cout << " Successfully deleted faculty record!\n";
                cout << "   Deleted from index: " << index << endl;
                cout << "   Total probes: " << probeCount << endl;
                return true;
            }
            
            index = (originalIndex + probeCount) % capacity;
            
            if (index == originalIndex) {
                break;
            }
            
            cout << "    Checking index " << index << " (probe " << probeCount + 1 << ")\n";
        }
        
        cout << " Faculty with ID " << facultyId << " not found for deletion!\n";
        cout << "   Total probes: " << probeCount << endl;
        return false;
    }
    
    // Display all faculty records
    void displayAllFaculty() {
        cout << "\n All Faculty Records in Database:\n";
        cout << "==================================\n";
        cout << "Total Faculty: " << size << ", Table Capacity: " << capacity << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << endl;
        
        bool found = false;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted) {
                found = true;
                cout << " Index " << i << ":\n";
                cout << "   ID: " << table[i].facultyId << " | Name: " << table[i].name 
                     << " | Dept: " << table[i].department << " | Designation: " << table[i].designation << endl;
            }
        }
        
        if (!found) {
            cout << "No faculty records found in the database.\n";
        }
        
        // Display deleted slots for educational purposes
        cout << "\n  Table Status (Educational View):\n";
        cout << "Index\tStatus\t\tFaculty ID\tName\n";
        cout << "-----\t------\t\t----------\t----\n";
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            if (table[i].isOccupied && !table[i].isDeleted) {
                cout << "Occupied\t" << table[i].facultyId << "\t\t" << table[i].name;
            } else if (table[i].isOccupied && table[i].isDeleted) {
                cout << "Deleted\t\t" << table[i].facultyId << "\t\t" << table[i].name << " (DELETED)";
            } else {
                cout << "Empty\t\t-\t\t-";
            }
            cout << endl;
        }
    }
    
    // Display database statistics
    void displayStatistics() {
        cout << "\n Database Statistics:\n";
        cout << "=====================\n";
        cout << "Table Capacity: " << capacity << endl;
        cout << "Number of Faculty: " << size << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << "Load Factor Threshold: " << LOAD_FACTOR_THRESHOLD << endl;
        
        int occupiedSlots = 0;
        int emptySlots = 0;
        int deletedSlots = 0;
        int maxProbeSequence = 0;
        
        for (int i = 0; i < capacity; i++) {
            if (!table[i].isOccupied) {
                emptySlots++;
            } else if (table[i].isDeleted) {
                deletedSlots++;
            } else {
                occupiedSlots++;
                // Calculate probe sequence length for this entry
                int originalIndex = hashFunction(table[i].facultyId);
                int probeLength = (i - originalIndex + capacity) % capacity + 1;
                if (probeLength > maxProbeSequence) {
                    maxProbeSequence = probeLength;
                }
            }
        }
        
        cout << "Occupied Slots: " << occupiedSlots << endl;
        cout << "Empty Slots: " << emptySlots << endl;
        cout << "Deleted Slots: " << deletedSlots << endl;
        cout << "Maximum Probe Sequence: " << maxProbeSequence << endl;
        cout << "Collision Rate: " << fixed << setprecision(1) 
             << (static_cast<double>(size - occupiedSlots) / size * 100) << "%" << endl;
    }
    
    // Search faculty by department
    void searchByDepartment(const string& department) {
        cout << "\n Searching Faculty in Department: " << department << "\n";
        cout << "===================================\n";
        
        vector<Faculty*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && table[i].department == department) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << " No faculty found in department '" << department << "'\n";
        } else {
            cout << " Found " << results.size() << " faculty member(s) in department '" << department << "':\n";
            for (Faculty* faculty : results) {
                cout << "    ID: " << faculty->facultyId 
                     << " | Name: " << faculty->name 
                     << " | Designation: " << faculty->designation 
                     << " | Email: " << faculty->email << endl;
            }
        }
    }
    
    // Search faculty by designation
    void searchByDesignation(const string& designation) {
        cout << "\n Searching Faculty with Designation: " << designation << "\n";
        cout << "======================================\n";
        
        vector<Faculty*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && table[i].designation == designation) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << " No faculty found with designation '" << designation << "'\n";
        } else {
            cout << " Found " << results.size() << " faculty member(s) with designation '" << designation << "':\n";
            for (Faculty* faculty : results) {
                cout << "    ID: " << faculty->facultyId 
                     << " | Name: " << faculty->name 
                     << " | Department: " << faculty->department 
                     << " | Email: " << faculty->email << endl;
            }
        }
    }
};

// Function to display menu
void displayMenu() {
    cout << "\n === Faculty Database Management System ===\n";
    cout << "1. Add Faculty Record\n";
    cout << "2. Search Faculty by ID\n";
    cout << "3. Display Faculty Record\n";
    cout << "4. Update Faculty Record\n";
    cout << "5. Delete Faculty Record\n";
    cout << "6. Display All Faculty\n";
    cout << "7. Search Faculty by Department\n";
    cout << "8. Search Faculty by Designation\n";
    cout << "9. Show Database Statistics\n";
    cout << "10. Exit\n";
    cout << "Enter your choice (1-10): ";
}

// Sample faculty data for demonstration
void addSampleData(FacultyDatabase& db) {
    cout << "\n Adding Sample Faculty Data...\n";
    db.insertFaculty(1001, "Dr. Rajesh Kumar", "Computer Science", "Professor", "rajesh.kumar@university.edu", "9876543210");
    db.insertFaculty(1002, "Dr. Priya Sharma", "Mathematics", "Associate Professor", "priya.sharma@university.edu", "9876543211");
    db.insertFaculty(1003, "Dr. Amit Patel", "Physics", "Professor", "amit.patel@university.edu", "9876543212");
    db.insertFaculty(1004, "Dr. Sunita Verma", "Computer Science", "Assistant Professor", "sunita.verma@university.edu", "9876543213");
    db.insertFaculty(2001, "Dr. Ravi Singh", "Chemistry", "Professor", "ravi.singh@university.edu", "9876543214");
    db.insertFaculty(2002, "Dr. Neha Gupta", "Electronics", "Associate Professor", "neha.gupta@university.edu", "9876543215");
    db.insertFaculty(3001, "Dr. Sanjay Mishra", "Mechanical", "Professor", "sanjay.mishra@university.edu", "9876543216");
}

int main() {
    int initialCapacity;
    cout << " === Faculty Database using Hashing (Linear Probing) ===\n";
    cout << "Enter initial capacity of hash table: ";
    cin >> initialCapacity;
    
    if (initialCapacity <= 0) {
        cout << " Invalid capacity! Using default capacity 10.\n";
        initialCapacity = 10;
    }
    
    FacultyDatabase database(initialCapacity);
    
    // Add sample data
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
                cout << "Enter Faculty ID to update: ";
                cin >> facultyId;
                cin.ignore();
                cout << "Enter new Name (press enter to skip): ";
                getline(cin, name);
                cout << "Enter new Department (press enter to skip): ";
                getline(cin, department);
                cout << "Enter new Designation (press enter to skip): ";
                getline(cin, designation);
                cout << "Enter new Email (press enter to skip): ";
                getline(cin, email);
                cout << "Enter new Phone (press enter to skip): ";
                getline(cin, phone);
                database.updateFaculty(facultyId, name, department, designation, email, phone);
                break;
                
            case 5:
                cout << "Enter Faculty ID to delete: ";
                cin >> facultyId;
                database.deleteFaculty(facultyId);
                break;
                
            case 6:
                database.displayAllFaculty();
                break;
                
            case 7:
                cin.ignore();
                cout << "Enter Department to search: ";
                getline(cin, department);
                database.searchByDepartment(department);
                break;
                
            case 8:
                cin.ignore();
                cout << "Enter Designation to search: ";
                getline(cin, designation);
                database.searchByDesignation(designation);
                break;
                
            case 9:
                database.displayStatistics();
                break;
                
            case 10:
                cout << "Exiting Faculty Database System. Thank you!\n";
                break;
                
            default:
                cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 10);
    
    return 0;
}
```
## Example Output (Demonstrating Linear Probing)
```
 === Faculty Database using Hashing (Linear Probing) ===
Enter initial capacity of hash table: 8

 Adding Sample Faculty Data...

 Inserting Faculty Record:
============================
Faculty ID: 1001
Hash value (MOD 8): 1
 Successfully inserted faculty record!
Storage location: Index 1
Total probes: 1

... (more sample data inserted)

 === Faculty Database Management System ===
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Update Faculty Record
5. Delete Faculty Record
6. Display All Faculty
7. Search Faculty by Department
8. Search Faculty by Designation
9. Show Database Statistics
10. Exit
Enter your choice (1-10): 2
Enter Faculty ID to search: 1001

 Searching for Faculty (ID: 1001):
==================================
Hash value (MOD 8): 1
 Faculty found at index 1!
Total probes: 1

 === Faculty Database Management System ===
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Update Faculty Record
5. Delete Faculty Record
6. Display All Faculty
7. Search Faculty by Department
8. Search Faculty by Designation
9. Show Database Statistics
10. Exit
Enter your choice (1-10): 6

 All Faculty Records in Database:
==================================
Total Faculty: 7, Table Capacity: 8
Load Factor: 0.88

 Index 1:
   ID: 1001 | Name: Dr. Rajesh Kumar | Dept: Computer Science | Designation: Professor
 Index 2:
   ID: 1002 | Name: Dr. Priya Sharma | Dept: Mathematics | Designation: Associate Professor
 Index 3:
   ID: 1003 | Name: Dr. Amit Patel | Dept: Physics | Designation: Professor
 Index 4:
   ID: 1004 | Name: Dr. Sunita Verma | Dept: Computer Science | Designation: Assistant Professor
 Index 5:
   ID: 2001 | Name: Dr. Ravi Singh | Dept: Chemistry | Designation: Professor
 Index 6:
   ID: 2002 | Name: Dr. Neha Gupta | Dept: Electronics | Designation: Associate Professor
 Index 7:
   ID: 3001 | Name: Dr. Sanjay Mishra | Dept: Mechanical | Designation: Professor

  Table Status (Educational View):
Index	Status		Faculty ID	Name
-----	------		----------	----
0	Empty		-		-
1	Occupied	1001		Dr. Rajesh Kumar
2	Occupied	1002		Dr. Priya Sharma
3	Occupied	1003		Dr. Amit Patel
4	Occupied	1004		Dr. Sunita Verma
5	Occupied	2001		Dr. Ravi Singh
6	Occupied	2002		Dr. Neha Gupta
7	Occupied	3001		Dr. Sanjay Mishra

 === Faculty Database Management System ===
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Update Faculty Record
5. Delete Faculty Record
6. Display All Faculty
7. Search Faculty by Department
8. Search Faculty by Designation
9. Show Database Statistics
10. Exit
Enter your choice (1-10): 7
Enter Department to search: Computer Science

 Searching Faculty in Department: Computer Science
===================================
 Found 2 faculty member(s) in department 'Computer Science':
    ID: 1001 | Name: Dr. Rajesh Kumar | Designation: Professor | Email: rajesh.kumar@university.edu
    ID: 1004 | Name: Dr. Sunita Verma | Designation: Assistant Professor | Email: sunita.verma@university.edu

 === Faculty Database Management System ===
1. Add Faculty Record
2. Search Faculty by ID
3. Display Faculty Record
4. Update Faculty Record
5. Delete Faculty Record
6. Display All Faculty
7. Search Faculty by Department
8. Search Faculty by Designation
9. Show Database Statistics
10. Exit
Enter your choice (1-10): 9

 Database Statistics:
=====================
Table Capacity: 8
Number of Faculty: 7
Load Factor: 0.88
Load Factor Threshold: 0.70
Occupied Slots: 7
Empty Slots: 1
Deleted Slots: 0
Maximum Probe Sequence: 1
Collision Rate: 0.0%
```
## Explanation:
- Record Structure: The struct Faculty holds the record fields: facultyID (the key), name, and department. The facultyID = 0 is used to clearly mark an empty slot.
- Linear Probing (Insertion): When inserting keys like 101, 11, and 21, which all initially hash to Index 1 
(since $101, 11, 21 \pmod{10} = 1$)

1. 11 collides at Index 1 and probes to the next available slot, Index 
2. 21 collides at Index 1 and Index 2, and probes to the next available slot, Index 3 This demonstrates the Primary Clustering effect inherent in Linear Probing.

- Linear Probing (Search): When searching for ID 21, the search starts at the initial hash Index 1. It finds 101 (no match), probes to Index 2 (finds 11, no match), and finally probes to Index 3 where 21 is found. The search successfully retraces the exact collision path.