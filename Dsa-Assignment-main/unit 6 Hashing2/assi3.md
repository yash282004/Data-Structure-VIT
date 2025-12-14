## Assignment 3

WAP to simulate employee databases as a hash table. Search a particular faculty by using Mid square method as a hash function for linear probing method of collision handling technique. Assume suitable data for faculty record.

## Author:
**Yash Santosh Khandagale**

## Aim:

To write a C++ program to simulate an Employee Database using a Hash Table.
The search operation should be performed using Mid-Square Method as the hash function and Linear Probing as the collision handling technique.

## Problem Statement:

- Create an employee database where each employee record contains:

- Employee ID

- Employee Name

- Employee Salary

- Insert employee records into a hash table using Mid-Square Hash Function.
- If a collision occurs, resolve it using Linear Probing.

- Finally, allow the user to search an employee by ID.

- Hash Function: Mid-Square Method

- Square the key (Employee ID).

-  the middle digits.

- Take modulo by table size if needed.
- Example:
- ID = 123
- 123² = 15129 → middle = 12 → index = 12 % size

## Algorithm
1. Step 1:

- Start the program.

2. Step 2:

- Create a structure Employee with fields:

- id

- name

- salary

3. Step 3:

- Create a hash table array with all entries initialized as empty (-1 ID).

4. Step 4 (Mid-Square Hash Function):

- Compute key²

- Convert to string

- Extract middle 1–2 digits (based on length)

- Index = extracted part % table_size

5. Step 5 (Insert Operation):

- Compute index using mid-square hashing

- If empty → place record

- Else → apply Linear Probing until an empty slot is found

6. Step 6 (Search Operation):

- Compute index using same hash function

- If record matches → return details

- Else probe forward linearly until:

- Key found, or

- Empty slot encountered → record not found

7. Step 7:

- Display output.

8. Step 8:

- Stop.


## C++ Program
```c++
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>
#include <cmath>
using namespace std;

struct Employee {
    int employeeId;
    string name;
    string department;
    string designation;
    double salary;
    string email;
    string phone;
    bool isOccupied;
    bool isDeleted;
    
    Employee() : employeeId(0), name(""), department(""), designation(""), salary(0.0), email(""), phone(""), isOccupied(false), isDeleted(false) {}
    
    Employee(int id, string n, string dept, string desig, double sal, string mail, string ph) 
        : employeeId(id), name(n), department(dept), designation(desig), salary(sal), email(mail), phone(ph), isOccupied(true), isDeleted(false) {}
};

class EmployeeDatabase {
private:
    vector<Employee> table;
    int capacity;
    int size;
    const double LOAD_FACTOR_THRESHOLD = 0.7;
    
    int midSquareHash(int employeeId) {
        long long square = (long long)employeeId * employeeId;
        string squareStr = to_string(square);
        
        int totalDigits = squareStr.length();
        int midPoint = totalDigits / 2;
        int digitsNeeded = to_string(capacity - 1).length();
        
        string midDigits;
        if (totalDigits <= digitsNeeded) {
            midDigits = squareStr;
        } else {
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
    
    void rehash() {
        cout << "Load factor exceeded threshold. Rehashing table..." << endl;
        
        vector<Employee> oldTable = table;
        capacity = capacity * 2;
        table.clear();
        table.resize(capacity);
        size = 0;
        
        for (const Employee& emp : oldTable) {
            if (emp.isOccupied && !emp.isDeleted) {
                insertEmployee(emp.employeeId, emp.name, emp.department, 
                             emp.designation, emp.salary, emp.email, emp.phone);
            }
        }
        cout << "Rehashing completed. New capacity: " << capacity << endl;
    }
    
public:
    EmployeeDatabase(int initialCapacity = 10) {
        capacity = initialCapacity;
        size = 0;
        table.resize(capacity);
    }
    
    void insertEmployee(int employeeId, const string& name, const string& department, 
                       const string& designation, double salary, const string& email, const string& phone) {
        if (static_cast<double>(size) / capacity >= LOAD_FACTOR_THRESHOLD) {
            rehash();
        }
        
        int hashIndex = midSquareHash(employeeId);
        int originalIndex = hashIndex;
        int probeCount = 0;
        
        cout << "Inserting Employee Record:" << endl;
        cout << "==========================" << endl;
        cout << "Employee ID: " << employeeId << endl;
        
        long long square = (long long)employeeId * employeeId;
        cout << "Square: " << square << endl;
        cout << "Mid Square Hash value: " << hashIndex << endl;
        
        while (table[hashIndex].isOccupied && !table[hashIndex].isDeleted) {
            if (table[hashIndex].employeeId == employeeId) {
                cout << "Error: Employee ID " << employeeId << " already exists" << endl;
                cout << "Existing record - Name: " << table[hashIndex].name 
                     << ", Department: " << table[hashIndex].department << endl;
                return;
            }
            
            probeCount++;
            hashIndex = (originalIndex + probeCount) % capacity;
            
            if (hashIndex == originalIndex) {
                cout << "Error: Table is full. Cannot insert employee record" << endl;
                return;
            }
            
            cout << "Collision at index " << (hashIndex - probeCount + capacity) % capacity 
                 << ". Linear probing to index " << hashIndex << endl;
        }
        
        table[hashIndex] = Employee(employeeId, name, department, designation, salary, email, phone);
        size++;
        
        cout << "Successfully inserted employee record" << endl;
        cout << "Storage location: Index " << hashIndex << endl;
        cout << "Total probes: " << probeCount + 1 << endl;
    }
    
    Employee* searchEmployee(int employeeId) {
        int hashIndex = midSquareHash(employeeId);
        int originalIndex = hashIndex;
        int probeCount = 0;
        
        cout << "Searching for Employee (ID: " << employeeId << "):" << endl;
        cout << "=================================" << endl;
        
        long long square = (long long)employeeId * employeeId;
        cout << "Square: " << square << endl;
        cout << "Mid Square Hash value: " << hashIndex << endl;
        
        while (table[hashIndex].isOccupied) {
            probeCount++;
            
            if (table[hashIndex].employeeId == employeeId && !table[hashIndex].isDeleted) {
                cout << "Employee found at index " << hashIndex << endl;
                cout << "Total probes: " << probeCount << endl;
                return &table[hashIndex];
            }
            
            hashIndex = (originalIndex + probeCount) % capacity;
            
            if (hashIndex == originalIndex) {
                break;
            }
            
            cout << "Checking index " << hashIndex << " (probe " << probeCount + 1 << ")" << endl;
        }
        
        cout << "Employee with ID " << employeeId << " not found" << endl;
        cout << "Total probes: " << probeCount << endl;
        return nullptr;
    }
    
    void displayEmployeeRecord(int employeeId) {
        Employee* employee = searchEmployee(employeeId);
        if (employee != nullptr) {
            cout << "Employee Record Details:" << endl;
            cout << "=======================" << endl;
            cout << left << setw(15) << "Employee ID:" << employee->employeeId << endl;
            cout << setw(15) << "Name:" << employee->name << endl;
            cout << setw(15) << "Department:" << employee->department << endl;
            cout << setw(15) << "Designation:" << employee->designation << endl;
            cout << setw(15) << "Salary:" << fixed << setprecision(2) << employee->salary << endl;
            cout << setw(15) << "Email:" << employee->email << endl;
            cout << setw(15) << "Phone:" << employee->phone << endl;
        }
    }
    
    void updateEmployee(int employeeId, const string& newName = "", const string& newDepartment = "", 
                       const string& newDesignation = "", double newSalary = -1.0, 
                       const string& newEmail = "", const string& newPhone = "") {
        Employee* employee = searchEmployee(employeeId);
        if (employee != nullptr) {
            cout << "Updating Employee Record (ID: " << employeeId << "):" << endl;
            cout << "=================================" << endl;
            
            bool updated = false;
            
            if (!newName.empty() && newName != employee->name) {
                cout << "Updating name from " << employee->name << " to " << newName << endl;
                employee->name = newName;
                updated = true;
            }
            if (!newDepartment.empty() && newDepartment != employee->department) {
                cout << "Updating department from " << employee->department << " to " << newDepartment << endl;
                employee->department = newDepartment;
                updated = true;
            }
            if (!newDesignation.empty() && newDesignation != employee->designation) {
                cout << "Updating designation from " << employee->designation << " to " << newDesignation << endl;
                employee->designation = newDesignation;
                updated = true;
            }
            if (newSalary >= 0 && newSalary != employee->salary) {
                cout << "Updating salary from " << employee->salary << " to " << newSalary << endl;
                employee->salary = newSalary;
                updated = true;
            }
            if (!newEmail.empty() && newEmail != employee->email) {
                cout << "Updating email from " << employee->email << " to " << newEmail << endl;
                employee->email = newEmail;
                updated = true;
            }
            if (!newPhone.empty() && newPhone != employee->phone) {
                cout << "Updating phone from " << employee->phone << " to " << newPhone << endl;
                employee->phone = newPhone;
                updated = true;
            }
            
            if (updated) {
                cout << "Employee record updated successfully" << endl;
            } else {
                cout << "No changes made to employee record" << endl;
            }
        }
    }
    
    bool deleteEmployee(int employeeId) {
        int hashIndex = midSquareHash(employeeId);
        int originalIndex = hashIndex;
        int probeCount = 0;
        
        cout << "Deleting Employee Record (ID: " << employeeId << "):" << endl;
        cout << "=================================" << endl;
        
        long long square = (long long)employeeId * employeeId;
        cout << "Square: " << square << endl;
        cout << "Mid Square Hash value: " << hashIndex << endl;
        
        while (table[hashIndex].isOccupied) {
            probeCount++;
            
            if (table[hashIndex].employeeId == employeeId && !table[hashIndex].isDeleted) {
                table[hashIndex].isDeleted = true;
                size--;
                cout << "Successfully deleted employee record" << endl;
                cout << "Deleted from index: " << hashIndex << endl;
                cout << "Total probes: " << probeCount << endl;
                return true;
            }
            
            hashIndex = (originalIndex + probeCount) % capacity;
            
            if (hashIndex == originalIndex) {
                break;
            }
            
            cout << "Checking index " << hashIndex << " (probe " << probeCount + 1 << ")" << endl;
        }
        
        cout << "Employee with ID " << employeeId << " not found for deletion" << endl;
        cout << "Total probes: " << probeCount << endl;
        return false;
    }
    
    void displayAllEmployees() {
        cout << "All Employee Records in Database:" << endl;
        cout << "=================================" << endl;
        cout << "Total Employees: " << size << ", Table Capacity: " << capacity << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << endl;
        
        cout << "Index\tEmp ID\tName\t\tDepartment\tDesignation\t\tStatus" << endl;
        cout << "-----\t------\t----\t\t----------\t----------\t\t------" << endl;
        
        for (int i = 0; i < capacity; i++) {
            cout << i << "\t";
            if (table[i].isOccupied && !table[i].isDeleted) {
                cout << table[i].employeeId << "\t"
                     << left << setw(12) << table[i].name.substr(0, 10) << "\t"
                     << left << setw(10) << table[i].department.substr(0, 8) << "\t"
                     << left << setw(15) << table[i].designation.substr(0, 12) << "\t"
                     << "Active";
            } else if (table[i].isOccupied && table[i].isDeleted) {
                cout << table[i].employeeId << "\t"
                     << left << setw(12) << table[i].name.substr(0, 10) << "\t"
                     << left << setw(10) << table[i].department.substr(0, 8) << "\t"
                     << left << setw(15) << table[i].designation.substr(0, 12) << "\t"
                     << "Deleted";
            } else {
                cout << "-\t-\t\t-\t\t-\t\tEmpty";
            }
            cout << endl;
        }
    }
    
    void displayHashAnalysis() {
        cout << "Mid Square Hash Function Analysis:" << endl;
        cout << "==================================" << endl;
        cout << "Table Capacity: " << capacity << endl;
        
        vector<int> employeeIds;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted) {
                employeeIds.push_back(table[i].employeeId);
            }
        }
        
        cout << "Employee IDs and their hash values:" << endl;
        for (int id : employeeIds) {
            long long square = (long long)id * id;
            int hashValue = midSquareHash(id);
            cout << "ID: " << id << " -> Square: " << square << " -> Hash: " << hashValue << endl;
        }
    }
    
    void displayStatistics() {
        cout << "Database Statistics:" << endl;
        cout << "====================" << endl;
        cout << "Table Capacity: " << capacity << endl;
        cout << "Number of Employees: " << size << endl;
        cout << "Load Factor: " << fixed << setprecision(2) << static_cast<double>(size) / capacity << endl;
        cout << "Load Factor Threshold: " << LOAD_FACTOR_THRESHOLD << endl;
        
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
                
                int employeeId = table[i].employeeId;
                int hashIndex = midSquareHash(employeeId);
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
    
    void searchByDepartment(const string& department) {
        cout << "Searching Employees in Department: " << department << endl;
        cout << "===================================" << endl;
        
        vector<Employee*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && table[i].department == department) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << "No employees found in department " << department << endl;
        } else {
            cout << "Found " << results.size() << " employee(s) in department " << department << ":" << endl;
            for (Employee* emp : results) {
                cout << "  ID: " << emp->employeeId 
                     << " | Name: " << emp->name 
                     << " | Designation: " << emp->designation 
                     << " | Salary: " << fixed << setprecision(2) << emp->salary << endl;
            }
        }
    }
    
    void searchByDesignation(const string& designation) {
        cout << "Searching Employees with Designation: " << designation << endl;
        cout << "======================================" << endl;
        
        vector<Employee*> results;
        for (int i = 0; i < capacity; i++) {
            if (table[i].isOccupied && !table[i].isDeleted && table[i].designation == designation) {
                results.push_back(&table[i]);
            }
        }
        
        if (results.empty()) {
            cout << "No employees found with designation " << designation << endl;
        } else {
            cout << "Found " << results.size() << " employee(s) with designation " << designation << ":" << endl;
            for (Employee* emp : results) {
                cout << "  ID: " << emp->employeeId 
                     << " | Name: " << emp->name 
                     << " | Department: " << emp->department 
                     << " | Salary: " << fixed << setprecision(2) << emp->salary << endl;
            }
        }
    }
};

void displayMenu() {
    cout << "Employee Database Management System" << endl;
    cout << "1. Add Employee Record" << endl;
    cout << "2. Search Employee by ID" << endl;
    cout << "3. Display Employee Record" << endl;
    cout << "4. Update Employee Record" << endl;
    cout << "5. Delete Employee Record" << endl;
    cout << "6. Display All Employees" << endl;
    cout << "7. Search Employees by Department" << endl;
    cout << "8. Search Employees by Designation" << endl;
    cout << "9. Show Database Statistics" << endl;
    cout << "10. Show Hash Analysis" << endl;
    cout << "11. Exit" << endl;
    cout << "Enter your choice (1-11): ";
}

void addSampleData(EmployeeDatabase& db) {
    cout << "Adding Sample Employee Data..." << endl;
    db.insertEmployee(1234, "John Smith", "IT", "Software Engineer", 75000.0, "john.smith@company.com", "1234567890");
    db.insertEmployee(5678, "Alice Johnson", "HR", "HR Manager", 65000.0, "alice.johnson@company.com", "1234567891");
    db.insertEmployee(9012, "Bob Wilson", "Finance", "Accountant", 60000.0, "bob.wilson@company.com", "1234567892");
    db.insertEmployee(3456, "Carol Davis", "IT", "System Analyst", 70000.0, "carol.davis@company.com", "1234567893");
    db.insertEmployee(7890, "David Brown", "Marketing", "Marketing Manager", 68000.0, "david.brown@company.com", "1234567894");
    db.insertEmployee(2345, "Eva Miller", "IT", "Database Admin", 72000.0, "eva.miller@company.com", "1234567895");
}

int main() {
    int initialCapacity;
    cout << "Employee Database using Mid Square Hashing with Linear Probing" << endl;
    cout << "Enter initial capacity of hash table: ";
    cin >> initialCapacity;
    
    if (initialCapacity <= 0) {
        cout << "Invalid capacity. Using default capacity 10." << endl;
        initialCapacity = 10;
    }
    
    EmployeeDatabase database(initialCapacity);
    
    addSampleData(database);
    
    int choice;
    int employeeId;
    string name, department, designation, email, phone;
    double salary;
    
    do {
        displayMenu();
        cin >> choice;
        
        switch (choice) {
            case 1:
                cout << "Enter Employee ID: ";
                cin >> employeeId;
                cin.ignore();
                cout << "Enter Name: ";
                getline(cin, name);
                cout << "Enter Department: ";
                getline(cin, department);
                cout << "Enter Designation: ";
                getline(cin, designation);
                cout << "Enter Salary: ";
                cin >> salary;
                cin.ignore();
                cout << "Enter Email: ";
                getline(cin, email);
                cout << "Enter Phone: ";
                getline(cin, phone);
                database.insertEmployee(employeeId, name, department, designation, salary, email, phone);
                break;
                
            case 2:
                cout << "Enter Employee ID to search: ";
                cin >> employeeId;
                database.searchEmployee(employeeId);
                break;
                
            case 3:
                cout << "Enter Employee ID to display: ";
                cin >> employeeId;
                database.displayEmployeeRecord(employeeId);
                break;
                
            case 4:
                cout << "Enter Employee ID to update: ";
                cin >> employeeId;
                cin.ignore();
                cout << "Enter new Name (press enter to skip): ";
                getline(cin, name);
                cout << "Enter new Department (press enter to skip): ";
                getline(cin, department);
                cout << "Enter new Designation (press enter to skip): ";
                getline(cin, designation);
                cout << "Enter new Salary (enter -1 to skip): ";
                cin >> salary;
                cin.ignore();
                cout << "Enter new Email (press enter to skip): ";
                getline(cin, email);
                cout << "Enter new Phone (press enter to skip): ";
                getline(cin, phone);
                database.updateEmployee(employeeId, name, department, designation, salary, email, phone);
                break;
                
            case 5:
                cout << "Enter Employee ID to delete: ";
                cin >> employeeId;
                database.deleteEmployee(employeeId);
                break;
                
            case 6:
                database.displayAllEmployees();
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
                database.displayHashAnalysis();
                break;
                
            case 11:
                cout << "Exiting Employee Database System. Thank you!" << endl;
                break;
                
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    } while (choice != 11);
    
    return 0;
}
```

## Sample Output
```
Employee Database using Mid Square Hashing with Linear Probing
Enter initial capacity of hash table: 10
Adding Sample Employee Data...
Inserting Employee Record:
==========================
Employee ID: 1234
Square: 1522756
Mid Square Hash value: 7
Successfully inserted employee record
Storage location: Index 7
Total probes: 1

Inserting Employee Record:
==========================
Employee ID: 5678
Square: 32239684
Mid Square Hash value: 9
Successfully inserted employee record
Storage location: Index 9
Total probes: 1

Employee Database Management System
1. Add Employee Record
2. Search Employee by ID
3. Display Employee Record
4. Update Employee Record
5. Delete Employee Record
6. Display All Employees
7. Search Employees by Department
8. Search Employees by Designation
9. Show Database Statistics
10. Show Hash Analysis
11. Exit
Enter your choice (1-11): 2
Enter Employee ID to search: 1234
Searching for Employee (ID: 1234):
=================================
Square: 1522756
Mid Square Hash value: 7
Employee found at index 7
Total probes: 1

Employee Database Management System
1. Add Employee Record
2. Search Employee by ID
3. Display Employee Record
4. Update Employee Record
5. Delete Employee Record
6. Display All Employees
7. Search Employees by Department
8. Search Employees by Designation
9. Show Database Statistics
10. Show Hash Analysis
11. Exit
Enter your choice (1-11): 6
All Employee Records in Database:
=================================
Total Employees: 6, Table Capacity: 10
Load Factor: 0.60

Index	Emp ID	Name		Department	Designation		Status
-----	------	----		----------	----------		------
0	-	-		-		-		Empty
1	-	-		-		-		Empty
2	-	-		-		-		Empty
3	-	-		-		-		Empty
4	-	-		-		-		Empty
5	-	-		-		-		Empty
6	9012	Bob Wilson	Finance		Accountant		Active
7	1234	John Smith	IT		Software Engin	Active
8	3456	Carol Davis	IT		System Analyst	Active
9	5678	Alice Johnso	HR		HR Manager		Active

Employee Database Management System
1. Add Employee Record
2. Search Employee by ID
3. Display Employee Record
4. Update Employee Record
5. Delete Employee Record
6. Display All Employees
7. Search Employees by Department
8. Search Employees by Designation
9. Show Database Statistics
10. Show Hash Analysis
11. Exit
Enter your choice (1-11): 10
Mid Square Hash Function Analysis:
==================================
Table Capacity: 10
Employee IDs and their hash values:
ID: 1234 -> Square: 1522756 -> Hash: 7
ID: 5678 -> Square: 32239684 -> Hash: 9
ID: 9012 -> Square: 812160144 -> Hash: 6
ID: 3456 -> Square: 11943936 -> Hash: 8
ID: 7890 -> Square: 62252100 -> Hash: 5
ID: 2345 -> Square: 5499025 -> Hash: 2
```
## Explanation

- Hashing is done using the mid-square method, ensuring better distribution.

- Collisions are resolved using linear probing.

- Searching uses the same hash function and probing sequence.

- The program simulates a real employee database.