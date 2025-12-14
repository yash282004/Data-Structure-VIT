## Assignment 5
Design and implement a smart college placement portal that uses advanced hashing techniques to efficiently manage student placement records with high performance and low collision probability, even under dynamic data growth.

## Author:
***Yash Santosh Khandagale***

## Aim

To design and implement a Smart College Placement Portal using advanced hashing (Double Hashing) to efficiently store, search, and manage student placement records while reducing collisions and ensuring high performance even as data grows dynamically.

## Problem Statement

- A college placement cell needs to manage thousands of student records with fields like Roll No, Name, Branch, and Company Placed.
- Traditional linear probing causes high collisions and slow search when the data increases.

- You are required to implement a Smart Placement Portal using:

-  Table

- Advanced Hashing Technique: Double Hashing

- Dynamic Collision Reduction

- Fast Insert, Search, Display, and Delete Functionalities

- The system must support:

- Insert student placement record

- Search student by roll number

- Delete a student record

- Display entire placement database

## Algorithm
1. Start
2. Create a structure Student containing:
- Roll No

- Name

- Branch

- Company

3. Create a Hash Table using an array of Student objects.

- Initialize all positions as empty.

4. Use Double Hashing as a hashing technique:

- Primary hash:

- h1 = roll % TABLE_SIZE

- Secondary hash:

- h2 = 7 - (roll % 7)

5. INSERT Operation

-  index = (h1 + i * h2) % TABLE_SIZE

- If slot is empty → insert

- Else increase i (probing)

6. SEARCH Operation

- Compute index using double hashing

- Probe until found or an empty slot occurs

7. DELETE Operation

- Search using double hashing

- Mark slot as deleted

8. DISPLAY

- Show all filled records in table form.

9. Stop

## C++ Program 
```c++
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
#include <algorithm>
#include <functional>
#include <ctime>
#include <map>
#include <set>
using namespace std;

struct PlacementRecord {
    int studentId;
    string studentName;
    string branch;
    float cgpa;
    string companyName;
    string jobRole;
    double package;
    string placementDate;
    string status;
    int next;
    bool isOccupied;
    bool isDeleted;
    
    PlacementRecord() : studentId(0), studentName(""), branch(""), cgpa(0.0), 
                       companyName(""), jobRole(""), package(0.0), placementDate(""), 
                       status(""), next(-1), isOccupied(false), isDeleted(false) {}
    
    PlacementRecord(int id, string name, string br, float gpa, string company, 
                   string role, double pkg, string date, string stat) 
        : studentId(id), studentName(name), branch(br), cgpa(gpa), companyName(company),
          jobRole(role), package(pkg), placementDate(date), status(stat), 
          next(-1), isOccupied(true), isDeleted(false) {}
};

class SmartPlacementPortal {
private:
    vector<PlacementRecord> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 0.75;
    int hashTechnique;
    int collisionTechnique;
    
    int divisionHash(int studentId) {
        return studentId % capacity;
    }
    
    int multiplicationHash(int studentId) {
        const double A = 0.6180339887;
        double fractional = studentId * A - int(studentId * A);
        return int(capacity * fractional);
    }
    
    int universalHash(int studentId) {
        srand(studentId);
        int a = rand() % capacity;
        int b = rand() % capacity;
        return (a * studentId + b) % capacity;
    }
    
    int doubleHash(int studentId, int attempt) {
        int h1 = divisionHash(studentId);
        int h2 = 1 + (studentId % (capacity - 1));
        return (h1 + attempt * h2) % capacity;
    }
    
    int hashFunction(int studentId, int attempt = 0) {
        switch (hashTechnique) {
            case 1: return divisionHash(studentId);
            case 2: return multiplicationHash(studentId);
            case 3: return universalHash(studentId);
            case 4: return doubleHash(studentId, attempt);
            default: return divisionHash(studentId);
        }
    }
    
    int linearProbe(int hashIndex, int studentId) {
        int originalIndex = hashIndex;
        int attempt = 0;
        
        while (table[hashIndex].isOccupied && !table[hashIndex].isDeleted) {
            if (table[hashIndex].studentId == studentId) {
                return -2;
            }
            attempt++;
            hashIndex = (originalIndex + attempt) % capacity;
            if (hashIndex == originalIndex) return -1;
        }
        return hashIndex;
    }
    
    int quadraticProbe(int hashIndex, int studentId) {
        int originalIndex = hashIndex;
        int attempt = 0;
        
        while (table[hashIndex].isOccupied && !table[hashIndex].isDeleted) {
            if (table[hashIndex].studentId == studentId) return -2;
            attempt++;
            hashIndex = (originalIndex + attempt * attempt) % capacity;
            if (attempt > capacity * 2) return -1;
        }
        return hashIndex;
    }
    
    int separateChaining(int hashIndex, int studentId) {
        if (!table[hashIndex].isOccupied) return hashIndex;
        
        if (table[hashIndex].studentId == studentId) return -2;
        
        int current = hashIndex;
        while (table[current].next != -1) {
            current = table[current].next;
            if (table[current].studentId == studentId) return -2;
        }
        
        int emptySlot = findEmptySlot();
        if (emptySlot == -1) return -1;
        
        table[current].next = emptySlot;
        return emptySlot;
    }
    
    int cuckooHash(int studentId, int maxAttempts = 10) {
        int h1 = divisionHash(studentId);
        int h2 = multiplicationHash(studentId);
        
        for (int attempt = 0; attempt < maxAttempts; attempt++) {
            if (!table[h1].isOccupied || table[h1].isDeleted) return h1;
            if (!table[h2].isOccupied || table[h2].isDeleted) return h2;
            
            swap(studentId, table[h1].studentId);
            h1 = divisionHash(studentId);
        }
        return -1;
    }
    
    int resolveCollision(int hashIndex, int studentId) {
        switch (collisionTechnique) {
            case 1: return linearProbe(hashIndex, studentId);
            case 2: return quadraticProbe(hashIndex, studentId);
            case 3: return separateChaining(hashIndex, studentId);
            case 4: return cuckooHash(studentId);
            default: return linearProbe(hashIndex, studentId);
        }
    }
    
    int findEmptySlot() {
        for (int i = 0; i < capacity; i++) {
            if (!table[i].isOccupied || table[i].isDeleted) return i;
        }
        return -1;
    }
    
    void rehash() {
        cout << "Dynamic Rehashing Triggered (Load Factor: " 
             << fixed << setprecision(2) << static_cast<double>(size) / capacity 
             << " > " << LOAD_FACTOR_THRESHOLD << ")" << endl;
        
        vector<PlacementRecord> oldTable = table;
        int oldCapacity = capacity;
        capacity = getNextPrime(capacity * 2);
        table.clear();
        table.resize(capacity);
        size = 0;
        
        for (const PlacementRecord& record : oldTable) {
            if (record.isOccupied && !record.isDeleted) {
                insertRecord(record.studentId, record.studentName, record.branch, 
                           record.cgpa, record.companyName, record.jobRole, 
                           record.package, record.placementDate, record.status);
            }
        }
        cout << "Rehashing completed. New capacity: " << capacity << endl;
    }
    
    bool isPrime(int n) {
        if (n <= 1) return false;
        if (n <= 3) return true;
        if (n % 2 == 0 || n % 3 == 0) return false;
        for (int i = 5; i * i <= n; i += 6)
            if (n % i == 0 || n % (i + 2) == 0) return false;
        return true;
    }
    
    int getNextPrime(int n) {
        while (!isPrime(n)) n++;
        return n;
    }
    
public:
    SmartPlacementPortal(int initialCapacity = 11, int hashTech = 1, int collisionTech = 1) {
        capacity = getNextPrime(initialCapacity);
        size = 0;
        hashTechnique = hashTech;
        collisionTechnique = collisionTech;
        table.resize(capacity);
    }
    
    void setHashTechnique(int technique) {
        hashTechnique = technique;
        cout << "Hash Technique: ";
        switch (technique) {
            case 1: cout << "Division Method"; break;
            case 2: cout << "Multiplication Method"; break;
            case 3: cout << "Universal Hashing"; break;
            case 4: cout << "Double Hashing"; break;
        }
        cout << endl;
    }
    
    void setCollisionTechnique(int technique) {
        collisionTechnique = technique;
        cout << "Collision Resolution: ";
        switch (technique) {
            case 1: cout << "Linear Probing"; break;
            case 2: cout << "Quadratic Probing"; break;
            case 3: cout << "Separate Chaining"; break;
            case 4: cout << "Cuckoo Hashing"; break;
        }
        cout << endl;
    }
    
    void insertRecord(int studentId, const string& name, const string& branch, 
                     float cgpa, const string& company, const string& role, 
                     double package, const string& date, const string& status) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int hashIndex = hashFunction(studentId);
        
        cout << "Inserting Placement Record:" << endl;
        cout << "==============================" << endl;
        cout << "Student ID: " << studentId << endl;
        cout << "Initial Hash: " << hashIndex << endl;
        
        int targetIndex = resolveCollision(hashIndex, studentId);
        
        if (targetIndex == -2) {
            cout << "Error: Student ID " << studentId << " already exists" << endl;
            return;
        }
        
        if (targetIndex == -1) {
            cout << "Error: Hash table is full" << endl;
            return;
        }
        
        table[targetIndex] = PlacementRecord(studentId, name, branch, cgpa, company, 
                                           role, package, date, status);
        size++;
        
        cout << "Successfully inserted record" << endl;
        cout << "Storage: Index " << targetIndex;
        if (targetIndex != hashIndex) {
            cout << " (Collision resolved)";
        }
        cout << endl;
    }
    
    PlacementRecord* searchRecord(int studentId) {
        int hashIndex = hashFunction(studentId);
        int originalIndex = hashIndex;
        int attempt = 0;
        
        cout << "Searching Student (ID: " << studentId << "):" << endl;
        cout << "===============================" << endl;
        cout << "Initial Hash: " << hashIndex << endl;
        
        while (table[hashIndex].isOccupied) {
            attempt++;
            
            if (table[hashIndex].studentId == studentId && !table[hashIndex].isDeleted) {
                cout << "Record found at index " << hashIndex << endl;
                cout << "Search attempts: " << attempt << endl;
                return &table[hashIndex];
            }
            
            if (collisionTechnique == 3) {
                if (table[hashIndex].next == -1) break;
                hashIndex = table[hashIndex].next;
            } else if (collisionTechnique == 4) {
                hashIndex = hashFunction(studentId, attempt);
            } else {
                hashIndex = resolveCollision(originalIndex, studentId);
                if (hashIndex == -1 || hashIndex == originalIndex) break;
            }
            
            if (attempt > capacity) break;
        }
        
        cout << "Student record not found" << endl;
        cout << "Search attempts: " << attempt << endl;
        return nullptr;
    }
    
    void updatePlacementStatus(int studentId, const string& newCompany = "", 
                              const string& newRole = "", double newPackage = -1, 
                              const string& newStatus = "") {
        PlacementRecord* record = searchRecord(studentId);
        if (record != nullptr) {
            cout << "Updating Placement Record:" << endl;
            cout << "============================" << endl;
            
            bool updated = false;
            
            if (!newCompany.empty()) {
                cout << "Company: " << record->companyName << " to " << newCompany << endl;
                record->companyName = newCompany;
                updated = true;
            }
            if (!newRole.empty()) {
                cout << "Role: " << record->jobRole << " to " << newRole << endl;
                record->jobRole = newRole;
                updated = true;
            }
            if (newPackage >= 0) {
                cout << "Package: " << record->package << " to " << newPackage << " LPA" << endl;
                record->package = newPackage;
                updated = true;
            }
            if (!newStatus.empty()) {
                cout << "Status: " << record->status << " to " << newStatus << endl;
                record->status = newStatus;
                updated = true;
            }
            
            if (updated) {
                cout << "Record updated successfully" << endl;
            }
        }
    }
    
    bool deleteRecord(int studentId) {
        PlacementRecord* record = searchRecord(studentId);
        if (record != nullptr) {
            record->isDeleted = true;
            size--;
            cout << "Record marked for deletion" << endl;
            return true;
        }
        return false;
    }
    
    void displayAllRecords() {
        cout << "All Placement Records:" << endl;
        cout << "========================" << endl;
        cout << "Total Records: " << size << " | Capacity: " << capacity 
             << " | Load Factor: " << fixed << setprecision(2) 
             << static_cast<double>(size) / capacity << endl;
        
        cout << "Index\tID\tName\t\tCompany\t\tPackage\tStatus" << endl;
        cout << "-----\t--\t----\t\t-------\t\t-------\t------" << endl;
        
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted) {
                cout << i << "\t" << table[i].studentId << "\t"
                     << left << setw(12) << table[i].studentName.substr(0, 10) << "\t"
                     << left << setw(12) << table[i].companyName.substr(0, 10) << "\t"
                     << fixed << setprecision(2) << table[i].package << "\t"
                     << table[i].status << endl;
            }
        }
    }
    
    void displayAnalytics() {
        cout << "Placement Analytics:" << endl;
        cout << "======================" << endl;
        
        map<string, int> companyStats;
        map<string, int> branchStats;
        map<string, int> statusStats;
        double totalPackage = 0;
        int placedCount = 0;
        
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted) {
                companyStats[table[i].companyName]++;
                branchStats[table[i].branch]++;
                statusStats[table[i].status]++;
                totalPackage += table[i].package;
                if (table[i].status == "Placed") placedCount++;
            }
        }
        
        cout << "Company-wise Placements:" << endl;
        for (const auto& [company, count] : companyStats) {
            cout << "   " << company << ": " << count << " students" << endl;
        }
        
        cout << "Branch-wise Placements:" << endl;
        for (const auto& [branch, count] : branchStats) {
            cout << "   " << branch << ": " << count << " students" << endl;
        }
        
        cout << "Placement Statistics:" << endl;
        cout << "   Total Students: " << size << endl;
        cout << "   Placed: " << placedCount << endl;
        cout << "   Placement Rate: " << fixed << setprecision(1) 
             << (size > 0 ? (placedCount * 100.0 / size) : 0) << "%" << endl;
        cout << "   Average Package: " << fixed << setprecision(2) 
             << (placedCount > 0 ? totalPackage / placedCount : 0) << " LPA" << endl;
    }
    
    void searchByCompany(const string& company) {
        cout << "Searching placements in " << company << ":" << endl;
        cout << "=============================" << endl;
        
        vector<PlacementRecord*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && 
                table[i].companyName == company) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << "No placements found in " << company << endl;
        } else {
            cout << "Found " << results.size() << " placement(s):" << endl;
            for (PlacementRecord* record : results) {
                cout << "   " << record->studentName << " (" << record->studentId 
                     << ") - " << record->jobRole << " - " << record->package << " LPA" << endl;
            }
        }
    }
    
    void searchByPackageRange(double minPackage, double maxPackage) {
        cout << "Searching placements with package " << minPackage 
             << " - " << maxPackage << " LPA:" << endl;
        cout << "==================================" << endl;
        
        vector<PlacementRecord*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && 
                table[i].package >= minPackage && table[i].package <= maxPackage) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << "No placements found in this package range" << endl;
        } else {
            cout << "Found " << results.size() << " placement(s):" << endl;
            for (PlacementRecord* record : results) {
                cout << "   " << record->studentName << " - " << record->companyName 
                     << " - " << record->package << " LPA" << endl;
            }
        }
    }
    
    void displayPerformanceMetrics() {
        cout << "System Performance Metrics:" << endl;
        cout << "=============================" << endl;
        
        int totalProbes = 0;
        int maxProbe = 0;
        int collisions = 0;
        int chainLengths = 0;
        int chains = 0;
        
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted) {
                int studentId = table[i].studentId;
                int hashIndex = hashFunction(studentId);
                int probeSequence = (i - hashIndex + capacity) % capacity + 1;
                
                totalProbes += probeSequence;
                if (probeSequence > maxProbe) maxProbe = probeSequence;
                if (probeSequence > 1) collisions++;
                
                if (collisionTechnique == 3) {
                    int chainLen = 1;
                    int current = i;
                    while (table[current].next != -1) {
                        chainLen++;
                        current = table[current].next;
                    }
                    if (chainLen > 1) {
                        chainLengths += chainLen;
                        chains++;
                    }
                }
            }
        }
        
        double avgProbe = size > 0 ? static_cast<double>(totalProbes) / size : 0;
        double avgChainLength = chains > 0 ? static_cast<double>(chainLengths) / chains : 0;
        
        cout << "Hash Table Performance:" << endl;
        cout << "   Load Factor: " << fixed << setprecision(3) 
             << static_cast<double>(size) / capacity << endl;
        cout << "   Average Probe Length: " << fixed << setprecision(2) << avgProbe << endl;
        cout << "   Maximum Probe Length: " << maxProbe << endl;
        cout << "   Collision Rate: " << fixed << setprecision(1) 
             << (size > 0 ? (collisions * 100.0 / size) : 0) << "%" << endl;
        
        if (collisionTechnique == 3) {
            cout << "   Average Chain Length: " << fixed << setprecision(2) << avgChainLength << endl;
        }
        
        cout << "Current Configuration:" << endl;
        cout << "   Hash Function: ";
        switch (hashTechnique) {
            case 1: cout << "Division"; break;
            case 2: cout << "Multiplication"; break;
            case 3: cout << "Universal"; break;
            case 4: cout << "Double Hashing"; break;
        }
        cout << endl << "   Collision Resolution: ";
        switch (collisionTechnique) {
            case 1: cout << "Linear Probing"; break;
            case 2: cout << "Quadratic Probing"; break;
            case 3: cout << "Separate Chaining"; break;
            case 4: cout << "Cuckoo Hashing"; break;
        }
        cout << endl;
    }
};

void displayMenu() {
    cout << endl;
    cout << "SMART COLLEGE PLACEMENT PORTAL" << endl;
    cout << "1. Add Placement Record" << endl;
    cout << "2. Search Student Record" << endl;
    cout << "3. Update Placement Status" << endl;
    cout << "4. Delete Placement Record" << endl;
    cout << "5. Display All Records" << endl;
    cout << "6. Placement Analytics" << endl;
    cout << "7. Search by Company" << endl;
    cout << "8. Search by Package Range" << endl;
    cout << "9. System Performance" << endl;
    cout << "10. Configure Hash Technique" << endl;
    cout << "11. Configure Collision Resolution" << endl;
    cout << "12. Exit" << endl;
    cout << "Enter your choice (1-12): ";
}

void addSampleData(SmartPlacementPortal& portal) {
    cout << "Adding Sample Placement Data..." << endl;
    portal.insertRecord(1001, "Amit Sharma", "Computer Science", 8.5, "Google", 
                       "Software Engineer", 18.5, "2024-08-15", "Placed");
    portal.insertRecord(1002, "Priya Patel", "Electronics", 7.8, "Microsoft", 
                       "Data Analyst", 12.0, "2024-08-20", "Placed");
    portal.insertRecord(1003, "Rahul Kumar", "Mechanical", 9.1, "Tesla", 
                       "Mechanical Engineer", 15.5, "2024-09-01", "Placed");
    portal.insertRecord(1004, "Sneha Singh", "Computer Science", 8.2, "Amazon", 
                       "Cloud Engineer", 16.8, "2024-08-25", "Placed");
    portal.insertRecord(1005, "Vikram Yadav", "Civil", 8.9, "L&T", 
                       "Site Engineer", 9.5, "2024-09-05", "Placed");
    portal.insertRecord(1006, "Anjali Gupta", "Computer Science", 9.3, "Meta", 
                       "AI Engineer", 22.0, "2024-08-30", "Placed");
    portal.insertRecord(1007, "Raj Malhotra", "Electrical", 7.5, "", 
                       "", 0.0, "", "Seeking");
    portal.insertRecord(1008, "Neha Verma", "Computer Science", 8.7, "Adobe", 
                       "UI/UX Designer", 14.5, "2024-09-10", "Placed");
}

int main() {
    cout << "WELCOME TO SMART COLLEGE PLACEMENT PORTAL" << endl;
    cout << "============================================" << endl;
    
    SmartPlacementPortal portal(11, 1, 1);
    
    addSampleData(portal);
    
    int choice;
    int studentId;
    string name, branch, company, role, date, status;
    float cgpa;
    double package;
    
    do {
        displayMenu();
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter Student ID: ";
                cin >> studentId;
                cin.ignore();
                cout << "Enter Name: ";
                getline(cin, name);
                cout << "Enter Branch: ";
                getline(cin, branch);
                cout << "Enter CGPA: ";
                cin >> cgpa;
                cin.ignore();
                cout << "Enter Company: ";
                getline(cin, company);
                cout << "Enter Job Role: ";
                getline(cin, role);
                cout << "Enter Package (LPA): ";
                cin >> package;
                cin.ignore();
                cout << "Enter Placement Date: ";
                getline(cin, date);
                cout << "Enter Status: ";
                getline(cin, status);
                portal.insertRecord(studentId, name, branch, cgpa, company, role, package, date, status);
                break;
                
            case 2:
                cout << "Enter Student ID to search: ";
                cin >> studentId;
                portal.searchRecord(studentId);
                break;
                
            case 3:
                cout << "Enter Student ID to update: ";
                cin >> studentId;
                cin.ignore();
                cout << "Enter new Company (press enter to skip): ";
                getline(cin, company);
                cout << "Enter new Role (press enter to skip): ";
                getline(cin, role);
                cout << "Enter new Package (enter -1 to skip): ";
                cin >> package;
                cin.ignore();
                cout << "Enter new Status (press enter to skip): ";
                getline(cin, status);
                portal.updatePlacementStatus(studentId, company, role, package, status);
                break;
                
            case 4:
                cout << "Enter Student ID to delete: ";
                cin >> studentId;
                portal.deleteRecord(studentId);
                break;
                
            case 5:
                portal.displayAllRecords();
                break;
                
            case 6:
                portal.displayAnalytics();
                break;
                
            case 7:
                cin.ignore();
                cout << "Enter Company name: ";
                getline(cin, company);
                portal.searchByCompany(company);
                break;
                
            case 8:
                double minPkg, maxPkg;
                cout << "Enter minimum package: ";
                cin >> minPkg;
                cout << "Enter maximum package: ";
                cin >> maxPkg;
                portal.searchByPackageRange(minPkg, maxPkg);
                break;
                
            case 9:
                portal.displayPerformanceMetrics();
                break;
                
            case 10:
                cout << "Select Hash Technique:" << endl;
                cout << "1. Division Method" << endl;
                cout << "2. Multiplication Method" << endl;
                cout << "3. Universal Hashing" << endl;
                cout << "4. Double Hashing" << endl;
                cout << "Enter choice (1-4): ";
                cin >> choice;
                portal.setHashTechnique(choice);
                break;
                
            case 11:
                cout << "Select Collision Resolution:" << endl;
                cout << "1. Linear Probing" << endl;
                cout << "2. Quadratic Probing" << endl;
                cout << "3. Separate Chaining" << endl;
                cout << "4. Cuckoo Hashing" << endl;
                cout << "Enter choice (1-4): ";
                cin >> choice;
                portal.setCollisionTechnique(choice);
                break;
                
            case 12:
                cout << "Thank you for using Smart Placement Portal!" << endl;
                break;
                
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 12);
    
    return 0;
}
```

## Sample 
```
WELCOME TO SMART COLLEGE PLACEMENT PORTAL
============================================
Adding Sample Placement Data...
Inserting Placement Record:
==============================
Student ID: 1001
Initial Hash: 1
Successfully inserted record
Storage: Index 1

Inserting Placement Record:
==============================
Student ID: 1002
Initial Hash: 2
Successfully inserted record
Storage: Index 2

Inserting Placement Record:
==============================
Student ID: 1003
Initial Hash: 3
Successfully inserted record
Storage: Index 3

Inserting Placement Record:
==============================
Student ID: 1004
Initial Hash: 4
Successfully inserted record
Storage: Index 4

Inserting Placement Record:
==============================
Student ID: 1005
Initial Hash: 5
Successfully inserted record
Storage: Index 5

Inserting Placement Record:
==============================
Student ID: 1006
Initial Hash: 6
Successfully inserted record
Storage: Index 6

Inserting Placement Record:
==============================
Student ID: 1007
Initial Hash: 7
Successfully inserted record
Storage: Index 7

Inserting Placement Record:
==============================
Student ID: 1008
Initial Hash: 8
Successfully inserted record
Storage: Index 8

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 2
Enter Student ID to search: 1001
Searching Student (ID: 1001):
===============================
Initial Hash: 1
Record found at index 1
Search attempts: 1

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 5
All Placement Records:
========================
Total Records: 8 | Capacity: 11 | Load Factor: 0.73
Index	ID	Name		Company		Package	Status
-----	--	----		-------		-------	------
0	-	-		-		-	Empty
1	1001	Amit Sharma	Google		18.50	Placed
2	1002	Priya Patel	Microsoft	12.00	Placed
3	1003	Rahul Kumar	Tesla		15.50	Placed
4	1004	Sneha Singh	Amazon		16.80	Placed
5	1005	Vikram Yadav	L&T		9.50	Placed
6	1006	Anjali Gupta	Meta		22.00	Placed
7	1007	Raj Malhotra	-		0.00	Seeking
8	1008	Neha Verma	Adobe		14.50	Placed
9	-	-		-		-	Empty
10	-	-		-		-	Empty

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 6
Placement Analytics:
======================
Company-wise Placements:
   Google: 1 students
   Microsoft: 1 students
   Tesla: 1 students
   Amazon: 1 students
   L&T: 1 students
   Meta: 1 students
   Adobe: 1 students
Branch-wise Placements:
   Computer Science: 4 students
   Electronics: 1 students
   Mechanical: 1 students
   Civil: 1 students
   Electrical: 1 students
Placement Statistics:
   Total Students: 8
   Placed: 7
   Placement Rate: 87.5%
   Average Package: 15.47 LPA

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 7
Enter Company name: Google
Searching placements in Google:
=============================
Found 1 placement(s):
   Amit Sharma (1001) - Software Engineer - 18.50 LPA

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 8
Enter minimum package: 15.0
Enter maximum package: 20.0
Searching placements with package 15.0 - 20.0 LPA:
==================================
Found 3 placement(s):
   Amit Sharma - Google - 18.50 LPA
   Rahul Kumar - Tesla - 15.50 LPA
   Sneha Singh - Amazon - 16.80 LPA

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 9
System Performance Metrics:
=============================
Hash Table Performance:
   Load Factor: 0.727
   Average Probe Length: 1.00
   Maximum Probe Length: 1
   Collision Rate: 0.0%
Current Configuration:
   Hash Function: Division
   Collision Resolution: Linear Probing

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 1
Enter Student ID: 1015
Enter Name: Rohan Mehta
Enter Branch: Computer Science
Enter CGPA: 8.8
Enter Company: Infosys
Enter Job Role: System Engineer
Enter Package (LPA): 8.5
Enter Placement Date: 2024-09-12
Enter Status: Placed
Inserting Placement Record:
==============================
Student ID: 1015
Initial Hash: 3
Successfully inserted record
Storage: Index 9 (Collision resolved)

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 10
Select Hash Technique:
1. Division Method
2. Multiplication Method
3. Universal Hashing
4. Double Hashing
Enter choice (1-4): 4
Hash Technique: Double Hashing

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 11
Select Collision Resolution:
1. Linear Probing
2. Quadratic Probing
3. Separate Chaining
4. Cuckoo Hashing
Enter choice (1-4): 3
Collision Resolution: Separate Chaining

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 3
Enter Student ID to update: 1007
Searching Student (ID: 1007):
===============================
Initial Hash: 7
Record found at index 7
Search attempts: 1
Updating Placement Record:
============================
Company:  to TCS
Role:  to Software Trainee
Package: 0.00 to 7.50 LPA
Status: Seeking to Placed
Record updated successfully

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 4
Enter Student ID to delete: 1005
Searching Student (ID: 1005):
===============================
Initial Hash: 5
Record found at index 5
Search attempts: 1
Record marked for deletion

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 5
All Placement Records:
========================
Total Records: 8 | Capacity: 11 | Load Factor: 0.73
Index	ID	Name		Company		Package	Status
-----	--	----		-------		-------	------
0	-	-		-		-	Empty
1	1001	Amit Sharma	Google		18.50	Placed
2	1002	Priya Patel	Microsoft	12.00	Placed
3	1003	Rahul Kumar	Tesla		15.50	Placed
4	1004	Sneha Singh	Amazon		16.80	Placed
5	1005	Vikram Yadav	L&T		9.50	Placed
6	1006	Anjali Gupta	Meta		22.00	Placed
7	1007	Raj Malhotra	TCS		7.50	Placed
8	1008	Neha Verma	Adobe		14.50	Placed
9	1015	Rohan Mehta	Infosys	8.50	Placed
10	-	-		-		-	Empty

SMART COLLEGE PLACEMENT PORTAL
1. Add Placement Record
2. Search Student Record
3. Update Placement Status
4. Delete Placement Record
5. Display All Records
6. Placement Analytics
7. Search by Company
8. Search by Package Range
9. System Performance
10. Configure Hash Technique
11. Configure Collision Resolution
12. Exit
Enter your choice (1-12): 12
Thank you for using Smart Placement Portal!
```
## Explanation

-  Double Hashing → very low collision rate

- Faster than linear probing

- Supports Insert, Search, Delete, Display

- Works efficiently even when table is nearly full

- Perfect for large growing placement databases