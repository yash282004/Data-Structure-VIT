## Assignment 4
*Title:*
WAP to simulate student databases as a hash table. a student database management system using hashing techniques to allow efficient insertion, search, and deletion of student records.

## Author:

**Yash Santosh Khandagale**

## Aim:

To write a C++ program that simulates a student database using hashing for efficient insertion, search, and deletion of student records.

## Problem Statement:

- Create a Student Database Management System using a hash table.
- Each student record contains:

- Roll Number

- Name

- Marks

- Use hashing to store records efficiently. Implement:

- Insertion

- Searching

- Deletion

- Use MOD hashing function with Linear Probing for collision resolution.

- Hash Function Used:
- index = rollNo % tableSize

## Algorithm:
1. Start the program
2. Define a structure Student

- Containing roll number, name, marks, and a flag indicating empty/filled/deleted.

3. Create a Hash Table

- Initialize all positions as empty.

4. For Insertion

- Compute index = rollNo % size

- If empty → insert

- If occupied → linear probe to next index

- Stop when record is inserted

5. For Searching

- Compute index

- Check the slot

- If roll number matches → student found

- Else probe until found or all checked

6. For Deletion

- Search for roll number

- If found → mark the slot as deleted

7. Display records
8. Stop

##  C++ Program
```c++
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
#include <algorithm>
using namespace std;

struct Student {
    int rollNumber;
    string name;
    string branch;
    int semester;
    float cgpa;
    string email;
    string phone;
    bool isOccupied;
    bool isDeleted;
    
    Student() : rollNumber(0), name(""), branch(""), semester(0), cgpa(0.0), email(""), phone(""), isOccupied(false), isDeleted(false) {}
    
    Student(int roll, string n, string br, int sem, float gpa, string mail, string ph) 
        : rollNumber(roll), name(n), branch(br), semester(sem), cgpa(gpa), email(mail), phone(ph), isOccupied(true), isDeleted(false) {}
};

class StudentDatabase {
private:
    vector<Student> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 0.7;
    int hashTechnique;
    
    int divisionHash(int rollNumber) {
        return rollNumber % capacity;
    }
    
    int midSquareHash(int rollNumber) {
        long long square = (long long)rollNumber * rollNumber;
        string squareStr = to_string(square);
        
        int totalDigits = squareStr.length();
        int digitsNeeded = to_string(capacity - 1).length();
        
        string midDigits;
        if (totalDigits <= digitsNeeded) {
            midDigits = squareStr;
        } else {
            int midPoint = totalDigits / 2;
            int start = midPoint - digitsNeeded / 2;
            if (start < 0) start = 0;
            int end = start + digitsNeeded;
            if (end > totalDigits) end = totalDigits;
            midDigits = squareStr.substr(start, end - start);
        }
        
        int hashValue = 0;
        if (!midDigits.empty()) {
            hashValue = stoi(midDigits) % capacity;
        }
        
        return hashValue;
    }
    
    int foldingHash(int rollNumber) {
        string rollStr = to_string(rollNumber);
        int sum = 0;
        int partSize = 2;
        
        for (size_t i = 0; i < rollStr.length(); i += partSize) {
            string part = rollStr.substr(i, partSize);
            sum += stoi(part);
        }
        
        return sum % capacity;
    }
    
    int hashFunction(int rollNumber) {
        switch (hashTechnique) {
            case 1: return divisionHash(rollNumber);
            case 2: return midSquareHash(rollNumber);
            case 3: return foldingHash(rollNumber);
            default: return divisionHash(rollNumber);
        }
    }
    
    void rehash() {
        cout << "Load factor exceeded threshold. Rehashing table..." << endl;
        
        vector<Student> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity);
        size = 0;
        
        for (const Student& student : oldTable) {
            if (student.isOccupied && !student.isDeleted) {
                insertStudent(student.rollNumber, student.name, student.branch, 
                            student.semester, student.cgpa, student.email, student.phone);
            }
        }
        cout << "Rehashing completed. New capacity: " << capacity << endl;
    }
    
    int linearProbe(int hashIndex, int rollNumber) {
        int originalIndex = hashIndex;
        int probeCount = 0;
        
        while (table[hashIndex].isOccupied && !table[hashIndex].isDeleted) {
            if (table[hashIndex].rollNumber == rollNumber) {
                return -2;
            }
            
            probeCount++;
            hashIndex = (originalIndex + probeCount) % capacity;
            
            if (hashIndex == originalIndex) {
                return -1;
            }
        }
        
        return hashIndex;
    }
    
public:
    StudentDatabase(int initialCapacity = 10, int technique = 1) {
        capacity = initialCapacity;
        size = 0;
        hashTechnique = technique;
        table.resize(capacity);
    }
    
    void setHashTechnique(int technique) {
        hashTechnique = technique;
        cout << "Hash technique changed to: ";
        switch (technique) {
            case 1: cout << "Division Method" << endl; break;
            case 2: cout << "Mid Square Method" << endl; break;
            case 3: cout << "Folding Method" << endl; break;
            default: cout << "Division Method" << endl;
        }
    }
    
    void insertStudent(int rollNumber, const string& name, const string& branch, 
                      int semester, float cgpa, const string& email, const string& phone) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int hashIndex = hashFunction(rollNumber);
        
        cout << "Inserting Student Record:" << endl;
        cout << "=========================" << endl;
        cout << "Roll Number: " << rollNumber << endl;
        cout << "Hash value: " << hashIndex << endl;
        
        if (hashTechnique == 2) {
            long long square = (long long)rollNumber * rollNumber;
            cout << "Square: " << square << endl;
        }
        
        int targetIndex = linearProbe(hashIndex, rollNumber);
        
        if (targetIndex == -2) {
            cout << "Error: Roll number " << rollNumber << " already exists" << endl;
            cout << "Existing record - Name: " << table[hashIndex].name 
                 << ", Branch: " << table[hashIndex].branch << endl;
            return;
        }
        
        if (targetIndex == -1) {
            cout << "Error: Table is full. Cannot insert student record" << endl;
            return;
        }
        
        table[targetIndex] = Student(rollNumber, name, branch, semester, cgpa, email, phone);
        size++;
        
        cout << "Successfully inserted student record" << endl;
        cout << "Storage location: Index " << targetIndex << endl;
        
        if (targetIndex != hashIndex) {
            cout << "Collision resolved using Linear Probing" << endl;
        }
    }
    
    Student* searchStudent(int rollNumber) {
        int hashIndex = hashFunction(rollNumber);
        int originalIndex = hashIndex;
        int probeCount = 0;
        
        cout << "Searching for Student (Roll Number: " << rollNumber << "):" << endl;
        cout << "===================================" << endl;
        cout << "Hash value: " << hashIndex << endl;
        
        if (hashTechnique == 2) {
            long long square = (long long)rollNumber * rollNumber;
            cout << "Square: " << square << endl;
        }
        
        while (table[hashIndex].isOccupied) {
            probeCount++;
            
            if (table[hashIndex].rollNumber == rollNumber && !table[hashIndex].isDeleted) {
                cout << "Student found at index " << hashIndex << endl;
                cout << "Total probes: " << probeCount << endl;
                return &table[hashIndex];
            }
            
            hashIndex = (originalIndex + probeCount) % capacity;
            
            if (hashIndex == originalIndex) {
                break;
            }
            
            if (table[hashIndex].isOccupied) {
                cout << "Checking index " << hashIndex << " (probe " << probeCount << ")" << endl;
            }
        }
        
        cout << "Student with roll number " << rollNumber << " not found" << endl;
        cout << "Total probes: " << probeCount << endl;
        return nullptr;
    }
    
    void displayStudentRecord(int rollNumber) {
        Student* student = searchStudent(rollNumber);
        if (student != nullptr) {
            cout << "Student Record Details:" << endl;
            cout << "======================" << endl;
            cout << left << setw(15) << "Roll Number:" << student->rollNumber << endl;
            cout << setw(15) << "Name:" << student->name << endl;
            cout << setw(15) << "Branch:" << student->branch << endl;
            cout << setw(15) << "Semester:" << student->semester << endl;
            cout << setw(15) << "CGPA:" << fixed << setprecision(2) << student->cgpa << endl;
            cout << setw(15) << "Email:" << student->email << endl;
            cout << setw(15) << "Phone:" << student->phone << endl;
        }
    }
    
    void updateStudent(int rollNumber, const string& newName = "", const string& newBranch = "", 
                      int newSemester = -1, float newCgpa = -1.0, 
                      const string& newEmail = "", const string& newPhone = "") {
        Student* student = searchStudent(rollNumber);
        if (student != nullptr) {
            cout << "Updating Student Record (Roll Number: " << rollNumber << "):" << endl;
            cout << "===================================" << endl;
            
            bool updated = false;
            
            if (!newName.empty() && newName != student->name) {
                cout << "Updating name from " << student->name << " to " << newName << endl;
                student->name = newName;
                updated = true;
            }
            if (!newBranch.empty() && newBranch != student->branch) {
                cout << "Updating branch from " << student->branch << " to " << newBranch << endl;
                student->branch = newBranch;
                updated = true;
            }
            if (newSemester != -1 && newSemester != student->semester) {
                cout << "Updating semester from " << student->semester << " to " << newSemester << endl;
                student->semester = newSemester;
                updated = true;
            }
            if (newCgpa >= 0 && newCgpa != student->cgpa) {
                cout << "Updating CGPA from " << student->cgpa << " to " << newCgpa << endl;
                student->cgpa = newCgpa;
                updated = true;
            }
            if (!newEmail.empty() && newEmail != student->email) {
                cout << "Updating email from " << student->email << " to " << newEmail << endl;
                student->email = newEmail;
                updated = true;
            }
            if (!newPhone.empty() && newPhone != student->phone) {
                cout << "Updating phone from " << student->phone << " to " << newPhone << endl;
                student->phone = newPhone;
                updated = true;
            }
            
            if (updated) {
                cout << "Student record updated successfully" << endl;
            } else {
                cout << "No changes made to student record" << endl;
            }
        }
    }
    
    bool deleteStudent(int rollNumber) {
        int hashIndex = hashFunction(rollNumber);
        int originalIndex = hashIndex;
        int probeCount = 0;
        
        cout << "Deleting Student Record (Roll Number: " << rollNumber << "):" << endl;
        cout << "===================================" << endl;
        cout << "Hash value: " << hashIndex << endl;
        
        while (table[hashIndex].isOccupied) {
            probeCount++;
            
            if (table[hashIndex].rollNumber == rollNumber && !table[hashIndex].isDeleted) {
                table[hashIndex].isDeleted = true;
                size--;
                cout << "Successfully deleted student record" << endl;
                cout << "Deleted from index: " << hashIndex << endl;
                cout << "Total probes: " << probeCount << endl;
                return true;
            }
            
            hashIndex = (originalIndex + probeCount) % capacity;
            
            if (hashIndex == originalIndex) {
                break;
            }
            
            if (table[hashIndex].isOccupied) {
                cout << "Checking index " << hashIndex << " (probe " << probeCount << ")" << endl;
            }
        }
        
        cout << "Student with roll number " << rollNumber << " not found for deletion" << endl;
        cout << "Total probes: " << probeCount << endl;
        return false;
    }
    
    void displayAllStudents() {
        cout << "All Student Records in Database:" << endl;
        cout << "================================" << endl;
        cout << "Total Students: " << size << ", Table Capacity: " << capacity << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << "Current Hash Technique: ";
        switch (hashTechnique) {
            case 1: cout << "Division Method"; break;
            case 2: cout << "Mid Square Method"; break;
            case 3: cout << "Folding Method"; break;
        }
        cout << endl << endl;
        
        cout << "Index\tRoll No\tName\t\tBranch\t\tSem\tCGPA\tStatus" << endl;
        cout << "-----\t-------\t----\t\t------\t\t---\t----\t------" << endl;
        
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            if (table[i].isOccupied && !table[i].isDeleted) {
                cout << table[i].rollNumber << "\t"
                     << left << setw(12) << table[i].name.substr(0, 10) << "\t"
                     << left << setw(10) << table[i].branch.substr(0, 8) << "\t"
                     << table[i].semester << "\t"
                     << fixed << setprecision(2) << table[i].cgpa << "\t"
                     << "Active";
            } else if (table[i].isOccupied && table[i].isDeleted) {
                cout << table[i].rollNumber << "\t"
                     << left << setw(12) << table[i].name.substr(0, 10) << "\t"
                     << left << setw(10) << table[i].branch.substr(0, 8) << "\t"
                     << table[i].semester << "\t"
                     << fixed << setprecision(2) << table[i].cgpa << "\t"
                     << "Deleted";
            } else {
                cout << "-\t-\t\t-\t\t-\t-\tEmpty";
            }
            cout << endl;
        }
    }
    
    void displayHashAnalysis() {
        cout << "Hash Function Analysis:" << endl;
        cout << "======================" << endl;
        cout << "Current Hash Technique: ";
        switch (hashTechnique) {
            case 1: cout << "Division Method"; break;
            case 2: cout << "Mid Square Method"; break;
            case 3: cout << "Folding Method"; break;
        }
        cout << endl;
        cout << "Table Capacity: " << capacity << endl;
        
        vector<int> rollNumbers;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted) {
                rollNumbers.push_back(table[i].rollNumber);
            }
        }
        
        cout << "Roll Numbers and their hash values:" << endl;
        for (int roll : rollNumbers) {
            int hashValue = hashFunction(roll);
            cout << "Roll: " << roll << " -> Hash: " << hashValue;
            if (hashTechnique == 2) {
                long long square = (long long)roll * roll;
                cout << " (Square: " << square << ")";
            }
            cout << endl;
        }
    }
    
    void displayStatistics() {
        cout << "Database Statistics:" << endl;
        cout << "====================" << endl;
        cout << "Table Capacity: " << capacity << endl;
        cout << "Number of Students: " << size << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << "Load Factor Threshold: " << LOAD_FACTOR_THRESHOLD << endl;
        cout << "Current Hash Technique: ";
        switch (hashTechnique) {
            case 1: cout << "Division Method"; break;
            case 2: cout << "Mid Square Method"; break;
            case 3: cout << "Folding Method"; break;
        }
        cout << endl;
        
        int occupiedSlots = 0;
        int emptySlots = 0;
        int deletedSlots = 0;
        int maxProbeSequence = 0;
        int totalProbes = 0;
        int searchCount = 0;
        
        for (int i = 0; i < capacity; i++) {
            if (!table[i].isOccupied) {
                emptySlots++;
            } else if (table[i].isDeleted) {
                deletedSlots++;
            } else {
                occupiedSlots++;
                
                int rollNumber = table[i].rollNumber;
                int hashIndex = hashFunction(rollNumber);
                int probeSequence = (i - hashIndex + capacity) % capacity + 1;
                totalProbes += probeSequence;
                searchCount++;
                
                if (probeSequence > maxProbeSequence) {
                    maxProbeSequence = probeSequence;
                }
            }
        }
        
        double avgProbeLength = searchCount > 0 ? static_cast<double>(totalProbes) / searchCount : 0;
        
        cout << "Occupied Slots: " << occupiedSlots << endl;
        cout << "Empty Slots: " << emptySlots << endl;
        cout << "Deleted Slots: " << deletedSlots << endl;
        cout << "Maximum Probe Sequence: " << maxProbeSequence << endl;
        cout << "Average Probe Sequence: " << fixed << setprecision(2) << avgProbeLength << endl;
        cout << "Collision Rate: " << fixed << setprecision(1) 
             << (static_cast<double>(size - occupiedSlots) / size * 100) << "%" << endl;
    }
    
    void searchByBranch(const string& branch) {
        cout << "Searching Students in Branch: " << branch << endl;
        cout << "==============================" << endl;
        
        vector<Student*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && table[i].branch == branch) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << "No students found in branch " << branch << endl;
        } else {
            cout << "Found " << results.size() << " student(s) in branch " << branch << ":" << endl;
            for (Student* student : results) {
                cout << "  Roll: " << student->rollNumber 
                     << " | Name: " << student->name 
                     << " | Semester: " << student->semester 
                     << " | CGPA: " << fixed << setprecision(2) << student->cgpa << endl;
            }
        }
    }
    
    void searchBySemester(int semester) {
        cout << "Searching Students in Semester: " << semester << endl;
        cout << "================================" << endl;
        
        vector<Student*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && table[i].semester == semester) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << "No students found in semester " << semester << endl;
        } else {
            cout << "Found " << results.size() << " student(s) in semester " << semester << ":" << endl;
            for (Student* student : results) {
                cout << "  Roll: " << student->rollNumber 
                     << " | Name: " << student->name 
                     << " | Branch: " << student->branch 
                     << " | CGPA: " << fixed << setprecision(2) << student->cgpa << endl;
            }
        }
    }
    
    void displayTopperByBranch(const string& branch) {
        cout << "Finding Topper in Branch: " << branch << endl;
        cout << "==========================" << endl;
        
        Student* topper = nullptr;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && table[i].branch == branch) {
                if (topper == nullptr || table[i].cgpa > topper->cgpa) {
                    topper = &table[i];
                }
            }
        }
        
        if (topper == nullptr) {
            cout << "No students found in branch " << branch << endl;
        } else {
            cout << "Topper in " << branch << " branch:" << endl;
            cout << "  Roll: " << topper->rollNumber << endl;
            cout << "  Name: " << topper->name << endl;
            cout << "  CGPA: " << fixed << setprecision(2) << topper->cgpa << endl;
            cout << "  Semester: " << topper->semester << endl;
        }
    }
};

void displayMenu() {
    cout << "Student Database Management System" << endl;
    cout << "1. Add Student Record" << endl;
    cout << "2. Search Student by Roll Number" << endl;
    cout << "3. Display Student Record" << endl;
    cout << "4. Update Student Record" << endl;
    cout << "5. Delete Student Record" << endl;
    cout << "6. Display All Students" << endl;
    cout << "7. Search Students by Branch" << endl;
    cout << "8. Search Students by Semester" << endl;
    cout << "9. Display Branch Topper" << endl;
    cout << "10. Change Hash Technique" << endl;
    cout << "11. Show Database Statistics" << endl;
    cout << "12. Show Hash Analysis" << endl;
    cout << "13. Exit" << endl;
    cout << "Enter your choice (1-13): ";
}

void addSampleData(StudentDatabase& db) {
    cout << "Adding Sample Student Data..." << endl;
    db.insertStudent(101, "Amit Sharma", "Computer Science", 3, 8.5, "amit.sharma@college.edu", "9876543210");
    db.insertStudent(102, "Priya Patel", "Electronics", 2, 7.8, "priya.patel@college.edu", "9876543211");
    db.insertStudent(103, "Rahul Kumar", "Mechanical", 4, 9.1, "rahul.kumar@college.edu", "9876543212");
    db.insertStudent(104, "Sneha Singh", "Computer Science", 3, 8.2, "sneha.singh@college.edu", "9876543213");
    db.insertStudent(105, "Vikram Yadav", "Civil", 2, 8.9, "vikram.yadav@college.edu", "9876543214");
    db.insertStudent(106, "Anjali Gupta", "Computer Science", 4, 9.3, "anjali.gupta@college.edu", "9876543215");
}

int main() {
    int initialCapacity, hashChoice;
    cout << "Student Database Management System using Hashing" << endl;
    cout << "Available Hash Techniques:" << endl;
    cout << "1. Division Method" << endl;
    cout << "2. Mid Square Method" << endl;
    cout << "3. Folding Method" << endl;
    cout << "Enter initial capacity of hash table: ";
    cin >> initialCapacity;
    cout << "Choose hash technique (1-3): ";
    cin >> hashChoice;
    
    if (initialCapacity <= 0) {
        cout << "Invalid capacity. Using default capacity 10." << endl;
        initialCapacity = 10;
    }
    if (hashChoice < 1 || hashChoice > 3) {
        cout << "Invalid choice. Using Division Method." << endl;
        hashChoice = 1;
    }
    
    StudentDatabase database(initialCapacity, hashChoice);
    
    addSampleData(database);
    
    int choice;
    int rollNumber, semester;
    string name, branch, email, phone;
    float cgpa;
    
    do {
        displayMenu();
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter Roll Number: ";
                cin >> rollNumber;
                cin.ignore();
                cout << "Enter Name: ";
                getline(cin, name);
                cout << "Enter Branch: ";
                getline(cin, branch);
                cout << "Enter Semester: ";
                cin >> semester;
                cout << "Enter CGPA: ";
                cin >> cgpa;
                cin.ignore();
                cout << "Enter Email: ";
                getline(cin, email);
                cout << "Enter Phone: ";
                getline(cin, phone);
                database.insertStudent(rollNumber, name, branch, semester, cgpa, email, phone);
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
                cout << "Enter new Branch (press enter to skip): ";
                getline(cin, branch);
                cout << "Enter new Semester (enter -1 to skip): ";
                cin >> semester;
                cout << "Enter new CGPA (enter -1 to skip): ";
                cin >> cgpa;
                cin.ignore();
                cout << "Enter new Email (press enter to skip): ";
                getline(cin, email);
                cout << "Enter new Phone (press enter to skip): ";
                getline(cin, phone);
                database.updateStudent(rollNumber, name, branch, semester, cgpa, email, phone);
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
                cout << "Enter Branch to search: ";
                getline(cin, branch);
                database.searchByBranch(branch);
                break;
                
            case 8:
                cout << "Enter Semester to search: ";
                cin >> semester;
                database.searchBySemester(semester);
                break;
                
            case 9:
                cin.ignore();
                cout << "Enter Branch to find topper: ";
                getline(cin, branch);
                database.displayTopperByBranch(branch);
                break;
                
            case 10:
                cout << "Choose Hash Technique:" << endl;
                cout << "1. Division Method" << endl;
                cout << "2. Mid Square Method" << endl;
                cout << "3. Folding Method" << endl;
                cout << "Enter choice (1-3): ";
                cin >> hashChoice;
                database.setHashTechnique(hashChoice);
                break;
                
            case 11:
                database.displayStatistics();
                break;
                
            case 12:
                database.displayHashAnalysis();
                break;
                
            case 13:
                cout << "Exiting Student Database System. Thank you!" << endl;
                break;
                
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 13);
    
    return 0;
}
```
## Sample Output
```
Student Database Management System using Hashing
Available Hash Techniques:
1. Division Method
2. Mid Square Method
3. Folding Method
Enter initial capacity of hash table: 10
Choose hash technique (1-3): 1
Adding Sample Student Data...
Inserting Student Record:
=========================
Roll Number: 101
Hash value: 1
Successfully inserted student record
Storage location: Index 1

Student Database Management System
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Branch
8. Search Students by Semester
9. Display Branch Topper
10. Change Hash Technique
11. Show Database Statistics
12. Show Hash Analysis
13. Exit
Enter your choice (1-13): 2
Enter Roll Number to search: 101
Searching for Student (Roll Number: 101):
===================================
Hash value: 1
Student found at index 1
Total probes: 1

Student Database Management System
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Branch
8. Search Students by Semester
9. Display Branch Topper
10. Change Hash Technique
11. Show Database Statistics
12. Show Hash Analysis
13. Exit
Enter your choice (1-13): 6
All Student Records in Database:
================================
Total Students: 6, Table Capacity: 10
Load Factor: 0.60
Current Hash Technique: Division Method

Index	Roll No	Name		Branch		Sem	CGPA	Status
-----	-------	----		------		---	----	------
0	-	-		-		-	-	Empty
1	101	Amit Sharma	Computer Sci	3	8.50	Active
2	102	Priya Patel	Electronics	2	7.80	Active
3	103	Rahul Kumar	Mechanical	4	9.10	Active
4	104	Sneha Singh	Computer Sci	3	8.20	Active
5	105	Vikram Yadav	Civil		2	8.90	Active
6	106	Anjali Gupta	Computer Sci	4	9.30	Active
7	-	-		-		-	-	Empty
8	-	-		-		-	-	Empty
9	-	-		-		-	-	Empty

Student Database Management System
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Branch
8. Search Students by Semester
9. Display Branch Topper
10. Change Hash Technique
11. Show Database Statistics
12. Show Hash Analysis
13. Exit
Enter your choice (1-13): 7
Enter Branch to search: Computer Science
Searching Students in Branch: Computer Science
==============================
Found 3 student(s) in branch Computer Science:
  Roll: 101 | Name: Amit Sharma | Semester: 3 | CGPA: 8.50
  Roll: 104 | Name: Sneha Singh | Semester: 3 | CGPA: 8.20
  Roll: 106 | Name: Anjali Gupta | Semester: 4 | CGPA: 9.30

Student Database Management System
1. Add Student Record
2. Search Student by Roll Number
3. Display Student Record
4. Update Student Record
5. Delete Student Record
6. Display All Students
7. Search Students by Branch
8. Search Students by Semester
9. Display Branch Topper
10. Change Hash Technique
11. Show Database Statistics
12. Show Hash Analysis
13. Exit
Enter your choice (1-13): 9
Enter Branch to find topper: Computer Science
Finding Topper in Branch: Computer Science
==========================
Topper in Computer Science branch:
  Roll: 106
  Name: Anjali Gupta
  CGPA: 9.30
  Semester: 4
```
## Explanation

- Hash table stores student records using roll number as key.

-  are resolved using linear probing.

-  probes sequentially until student is found.

- Deletion marks the slot as deleted but keeps probing functional.

- Hash table ensures fast access and efficient data management.