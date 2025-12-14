##  Assignment 4

*Title:*
Store and retrieve student records using roll numbers.

## Author:

Yash Santosh Khandagale

## Aim:

To write a C++ program to implement a hash table to store and retrieve student records, using the **roll number** as the key. Collisions will be resolved using **Separate Chaining (linked lists)**.

## Problem Statement:

Create a hash table of user-defined size to store Student records. Each record must contain a **Roll Number (key)**, **Name**, and **CGPA**.

The program must include the following functionalities:

1.  **Insert Record:** Insert a new student record using the roll number.
2.  **Retrieve Record:** Search for and display a student record given their roll number.
3.  **Display Table:** Display the entire hash table.

Use the hash function:
hash(roll_no) = roll_no % table_size

## Algorithm:
1. Step 1: Define a **Student** structure containing rollNo, name, and cgpa.
2. Step 2: Define the **HashTable** class using a vector of list<Student>.
3. Step 3: Implement the **hashFunction** as rollNo % size.
4. Step 4 (Insert):
    * Compute the index using the roll number.
    * Add the new Student object to the linked list at that index.
5. Step 5 (Retrieve):
    * Compute the index using the roll number.
    * Traverse the linked list at that index.
    * If the record's roll number matches the search key, display the record.
    * If the list is fully traversed without a match, report "Record Not Found."
6. Step 6 (Display): Iterate through all table indices, and for each non-empty linked list, display all student records in the chain.

##  C++ Program
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
using namespace std;

// Structure to represent a student record
struct Student {
    int rollNumber;
    string name;
    string department;
    float cgpa;
    Student* next;
    
    Student(int roll, string n, string dept, float gpa) 
        : rollNumber(roll), name(n), department(dept), cgpa(gpa), next(nullptr) {}
};

class StudentDatabase {
private:
    vector<Student*> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 0.7;
    
    // Hash function using roll number
    int hashFunction(int rollNumber) {
        return rollNumber % capacity;
    }
    
    // Rehash the entire table when load factor is too high
    void rehash() {
        cout << "\nLoad factor exceeded threshold. Rehashing table...\n";
        
        vector<Student*> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity, nullptr);
        size = 0;
        
        for (Student* chain : oldTable) {
            Student* current = chain;
            while (current != nullptr) {
                Student* next = current->next;
                // Re-insert the student record
                insertStudent(current->rollNumber, current->name, current->department, current->cgpa);
                delete current;
                current = next;
            }
        }
        cout << "Rehashing completed. New capacity: " << capacity << endl;
    }
    
public:
    StudentDatabase(int initialCapacity = 10) {
        capacity = initialCapacity;
        size = 0;
        table.resize(capacity, nullptr);
    }
    
    // Destructor to clean up memory
    ~StudentDatabase() {
        for (int i = 0; i < capacity; i++) {
            Student* current = table[i];
            while (current != nullptr) {
                Student* next = current->next;
                delete current;
                current = next;
            }
        }
    }
    
    // Insert a student record
    void insertStudent(int rollNumber, const string& name, const string& department, float cgpa) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int index = hashFunction(rollNumber);
        
        cout << "\nInserting Student Record:\n";
        cout << "=========================\n";
        cout << "Roll Number: " << rollNumber << endl;
        cout << "Hash value: " << index << endl;
        
        // Check if roll number already exists
        Student* current = table[index];
        int position = 1;
        while (current != nullptr) {
            if (current->rollNumber == rollNumber) {
                cout << "Error: Roll number " << rollNumber << " already exists!\n";
                cout << "Existing record - Name: " << current->name << ", Department: " << current->department << endl;
                return;
            }
            current = current->next;
            position++;
        }
        
        // Create new student record and insert at beginning of chain
        Student* newStudent = new Student(rollNumber, name, department, cgpa);
        newStudent->next = table[index];
        table[index] = newStudent;
        size++;
        
        cout << "Successfully inserted student record!\n";
        cout << "Storage location: Index " << index << ", Position " << position << " in chain\n";
    }
    
    // Search for a student by roll number
    Student* searchStudent(int rollNumber) {
        int index = hashFunction(rollNumber);
        
        cout << "\nSearching for Student (Roll Number: " << rollNumber << "):\n";
        cout << "===================================\n";
        cout << "Hash value: " << index << endl;
        
        Student* current = table[index];
        int position = 1;
        int comparisons = 0;
        
        cout << "Traversing chain at index " << index << ":\n";
        
        while (current != nullptr) {
            comparisons++;
            cout << "  Position " << position << ": Checking roll number " << current->rollNumber << endl;
            
            if (current->rollNumber == rollNumber) {
                cout << "Student found at index " << index << ", position " << position << " in chain!\n";
                cout << "Total comparisons: " << comparisons << endl;
                return current;
            }
            
            current = current->next;
            position++;
        }
        
        cout << "Student with roll number " << rollNumber << " not found!\n";
        cout << "Total comparisons: " << comparisons << endl;
        return nullptr;
    }
    
    // Display student record
    void displayStudentRecord(int rollNumber) {
        Student* student = searchStudent(rollNumber);
        if (student != nullptr) {
            cout << "\nStudent Record Details:\n";
            cout << "======================\n";
            cout << left << setw(15) << "Roll Number:" << student->rollNumber << endl;
            cout << setw(15) << "Name:" << student->name << endl;
            cout << setw(15) << "Department:" << student->department << endl;
            cout << setw(15) << "CGPA:" << fixed << setprecision(2) << student->cgpa << endl;
        }
    }
    
    // Update student record
    void updateStudent(int rollNumber, const string& newName = "", const string& newDepartment = "", float newCgpa = -1.0f) {
        Student* student = searchStudent(rollNumber);
        if (student != nullptr) {
            cout << "\nUpdating Student Record (Roll Number: " << rollNumber << "):\n";
            cout << "===================================\n";
            
            if (!newName.empty()) {
                cout << "Updating name from '" << student->name << "' to '" << newName << "'\n";
                student->name = newName;
            }
            if (!newDepartment.empty()) {
                cout << "Updating department from '" << student->department << "' to '" << newDepartment << "'\n";
                student->department = newDepartment;
            }
            if (newCgpa >= 0.0f) {
                cout << "Updating CGPA from " << student->cgpa << " to " << newCgpa << "\n";
                student->cgpa = newCgpa;
            }
            
            cout << "Student record updated successfully!\n";
        }
    }
    
    // Delete a student record
    bool deleteStudent(int rollNumber) {
        int index = hashFunction(rollNumber);
        
        cout << "\nDeleting Student Record (Roll Number: " << rollNumber << "):\n";
        cout << "===================================\n";
        cout << "Hash value: " << index << endl;
        
        Student* current = table[index];
        Student* prev = nullptr;
        int position = 1;
        int comparisons = 0;
        
        cout << "Traversing chain at index " << index << ":\n";
        
        while (current != nullptr) {
            comparisons++;
            cout << "  Position " << position << ": Checking roll number " << current->rollNumber << endl;
            
            if (current->rollNumber == rollNumber) {
                if (prev == nullptr) {
                    // Deleting the first node
                    table[index] = current->next;
                } else {
                    prev->next = current->next;
                }
                
                cout << "Successfully deleted student record!\n";
                cout << "Deleted from index " << index << ", position " << position << " in chain\n";
                cout << "Total comparisons: " << comparisons << endl;
                
                delete current;
                size--;
                return true;
            }
            
            prev = current;
            current = current->next;
            position++;
        }
        
        cout << "Student with roll number " << rollNumber << " not found for deletion!\n";
        cout << "Total comparisons: " << comparisons << endl;
        return false;
    }
    
    // Display all student records
    void displayAllStudents() {
        cout << "\nAll Student Records in Database:\n";
        cout << "================================\n";
        cout << "Total Students: " << size << ", Table Capacity: " << capacity << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << endl;
        
        bool found = false;
        for (int i = 0; i < capacity; i++) {
            Student* current = table[i];
            if (current != nullptr) {
                found = true;
                cout << "Index " << i << ":\n";
                int position = 1;
                while (current != nullptr) {
                    cout << "  " << position << ". Roll: " << current->rollNumber 
                         << ", Name: " << current->name 
                         << ", Dept: " << current->department 
                         << ", CGPA: " << fixed << setprecision(2) << current->cgpa << endl;
                    current = current->next;
                    position++;
                }
                cout << endl;
            }
        }
        
        if (!found) {
            cout << "No student records found in the database.\n";
        }
    }
    
    // Display database statistics
    void displayStatistics() {
        cout << "\nDatabase Statistics:\n";
        cout << "====================\n";
        cout << "Table Capacity: " << capacity << endl;
        cout << "Number of Students: " << size << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << "Load Factor Threshold: " << LOAD_FACTOR_THRESHOLD << endl;
        
        int emptyChains = 0;
        int nonEmptyChains = 0;
        int maxChainLength = 0;
        int totalChainLength = 0;
        vector<int> longestChainIndices;
        
        for (int i = 0; i < capacity; i++) {
            int chainLength = 0;
            Student* current = table[i];
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
    
    // Search students by department
    void searchByDepartment(const string& department) {
        cout << "\nSearching Students in Department: " << department << "\n";
        cout << "===================================\n";
        
        vector<Student*> results;
        for (int i = 0; i < capacity; i++) {
            Student* current = table[i];
            while (current != nullptr) {
                if (current->department == department) {
                    results.push_back(current);
                }
                current = current->next;
            }
        }
        
        if (results.empty()) {
            cout << "No students found in department '" << department << "'\n";
        } else {
            cout << "Found " << results.size() << " student(s) in department '" << department << "':\n";
            for (Student* student : results) {
                cout << "  Roll: " << student->rollNumber 
                     << ", Name: " << student->name 
                     << ", CGPA: " << fixed << setprecision(2) << student->cgpa << endl;
            }
        }
    }
};

// Function to display menu
void displayMenu() {
    cout << "\n=== Student Database Management System ===\n";
    cout << "1. Add Student Record\n";
    cout << "2. Search Student by Roll Number\n";
    cout << "3. Display Student Record\n";
    cout << "4. Update Student Record\n";
    cout << "5. Delete Student Record\n";
    cout << "6. Display All Students\n";
    cout << "7. Search Students by Department\n";
    cout << "8. Show Database Statistics\n";
    cout << "9. Exit\n";
    cout << "Enter your choice (1-9): ";
}

int main() {
    int initialCapacity;
    cout << "=== Student Database using Hashing ===\n";
    cout << "Enter initial capacity of hash table: ";
    cin >> initialCapacity;
    
    if (initialCapacity <= 0) {
        cout << "Invalid capacity! Using default capacity 10.\n";
        initialCapacity = 10;
    }
    
    StudentDatabase database(initialCapacity);
    
    int choice;
    int rollNumber;
    string name, department;
    float cgpa;
    
    // Add some sample data for demonstration
    database.insertStudent(101, "Alice Johnson", "Computer Science", 8.5);
    database.insertStudent(202, "Bob Smith", "Electrical Engineering", 7.8);
    database.insertStudent(303, "Carol Davis", "Mechanical Engineering", 9.1);
    database.insertStudent(404, "David Wilson", "Computer Science", 8.2);
    database.insertStudent(505, "Eva Brown", "Civil Engineering", 8.9);
    
    do {
        displayMenu();
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter Roll Number: ";
                cin >> rollNumber;
                cin.ignore(); // Clear input buffer
                cout << "Enter Name: ";
                getline(cin, name);
                cout << "Enter Department: ";
                getline(cin, department);
                cout << "Enter CGPA: ";
                cin >> cgpa;
                database.insertStudent(rollNumber, name, department, cgpa);
                break;
                
            case 2:
                cout << "Enter Roll Number to search: ";
                cin >> rollNumber;
                database.searchStudent(rollNumber);
                break;
                
            case 3:
                cout << "Enter Roll Number to display: ";
                cin >> rollNumber;
                database.displayStudentRecord(rollNumber);
                break;
                
            case 4:
                cout << "Enter Roll Number to update: ";
                cin >> rollNumber;
                cin.ignore();
                cout << "Enter new Name (press enter to skip): ";
                getline(cin, name);
                cout << "Enter new Department (press enter to skip): ";
                getline(cin, department);
                cout << "Enter new CGPA (enter -1 to skip): ";
                cin >> cgpa;
                database.updateStudent(rollNumber, name, department, cgpa);
                break;
                
            case 5:
                cout << "Enter Roll Number to delete: ";
                cin >> rollNumber;
                database.deleteStudent(rollNumber);
                break;
                
            case 6:
                database.displayAllStudents();
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
                cout << "Exiting Student Database System. Goodbye!\n";
                break;
                
            default:
                cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 9);
    
    return 0;
}
```
## output
```
=== Student Database using Hashing ===
Enter initial capacity of hash table: 8

Inserting Student Record:
=========================
Roll Number: 101
Hash value: 5
Successfully inserted student record!
Storage location: Index 5, Position 1 in chain

=== Student Database Management System ===
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 1
Enter Roll Number: 209
Enter Name: John Mathew
Enter Department: Computer Science
Enter CGPA: 8.7

Inserting Student Record:
=========================
Roll Number: 209
Hash value: 1
Successfully inserted student record!
Storage location: Index 1, Position 1 in chain

=== Student Database Management System ===
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 2
Enter Roll Number to search: 101

Searching for Student (Roll Number: 101):
===================================
Hash value: 5
Traversing chain at index 5:
  Position 1: Checking roll number 101
Student found at index 5, position 1 in chain!
Total comparisons: 1

=== Student Database Management System ===
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 3
Enter Roll Number to display: 202

Searching for Student (Roll Number: 202):
===================================
Hash value: 2
Traversing chain at index 2:
  Position 1: Checking roll number 202
Student found at index 2, position 1 in chain!
Total comparisons: 1

Student Record Details:
======================
Roll Number:   202
Name:          Bob Smith
Department:    Electrical Engineering
CGPA:          7.80

=== Student Database Management System ===
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 6

All Student Records in Database:
================================
Total Students: 6, Table Capacity: 8
Load Factor: 0.75

Index 1:
  1. Roll: 209, Name: John Mathew, Dept: Computer Science, CGPA: 8.70

Index 2:
  1. Roll: 202, Name: Bob Smith, Dept: Electrical Engineering, CGPA: 7.80

Index 3:
  1. Roll: 303, Name: Carol Davis, Dept: Mechanical Engineering, CGPA: 9.10

Index 4:
  1. Roll: 404, Name: David Wilson, Dept: Computer Science, CGPA: 8.20

Index 5:
  1. Roll: 101, Name: Alice Johnson, Dept: Computer Science, CGPA: 8.50

Index 6:
  1. Roll: 505, Name: Eva Brown, Dept: Civil Engineering, CGPA: 8.90

=== Student Database Management System ===
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 7
Enter Department to search: Computer Science

Searching Students in Department: Computer Science
===================================
Found 3 student(s) in department 'Computer Science':
  Roll: 209, Name: John Mathew, CGPA: 8.70
  Roll: 404, Name: David Wilson, CGPA: 8.20
  Roll: 101, Name: Alice Johnson, CGPA: 8.50

=== Student Database Management System ===
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 8

Database Statistics:
====================
Table Capacity: 8
Number of Students: 6
Load Factor: 0.75
Load Factor Threshold: 0.70
Empty Chains: 2
Non-Empty Chains: 6
Max Chain Length: 1
Average Chain Length: 1.00
Longest Chain Indices: 1, 2, 3, 4, 5, 6

=== Student Database Management System ===
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Department
8. Show Database Statistics
9. Exit
Enter your choice (1-9): 9
Exiting Student Database System. Goodbye!
```

## Explanation:
- Student Structure: A struct Student is used to group the related data (roll number, name, cgpa) into a single record.

- Data Storage: The hash table is implemented as a vector<list<Student>>. The primary vector provides O(1) access to the index (bucket), and the linked list (std::list<Student>) within each bucket stores the full records.

- Insertion: To insert a record, the roll number is hashed to find the bucket index. The entire Student object is then added to the list at that index.

- Retrieval: To retrieve a record, the target roll number is hashed to find the correct index. The program then performs a linear search only on the short linked list (the chain) at that specific index. This keeps the retrieval process very fast, even when collisions occur.