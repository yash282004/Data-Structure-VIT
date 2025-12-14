# Program by Vaidehi Vinayak Unhale

## Problem Statement / Title  
In Computer Engineering Department of VIT, there are S.Y., T.Y., and B.Tech students. Assume all these students are on the ground for a function. We need to identify a student of S.Y. Div. (X) whose name is "XYZ" and roll no. is "17". Apply an appropriate searching method to identify the required student.

## Description  
We are given a list of students from different classes. We want to search for a **specific student** (S.Y. Div. X, name = XYZ, roll no. = 17).  
Since the data might not be sorted, we use **Linear Search** to go through each student one by one until we find a match.

## Code  
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student_vvu {
    string name_vvu;
    int rollNo_vvu;
    string class_vvu; 
    string division_vvu;
};

int main() {
    int n_vvu;
    cout << "Enter total number of students: ";
    cin >> n_vvu;

    Student_vvu students_vvu[100];

    // Input details of students
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cout << "\nEnter details of student " << i_vvu + 1 << ":\n";
        cout << "Name: ";
        cin >> students_vvu[i_vvu].name_vvu;
        cout << "Roll Number: ";
        cin >> students_vvu[i_vvu].rollNo_vvu;
        cout << "Class (S.Y./T.Y./B.Tech): ";
        cin >> students_vvu[i_vvu].class_vvu;
        cout << "Division: ";
        cin >> students_vvu[i_vvu].division_vvu;
    }

    string searchName_vvu = "XYZ";
    int searchRoll_vvu = 17;
    string searchClass_vvu = "S.Y.";
    string searchDiv_vvu = "X";

    bool found_vvu = false;

    // Linear Search
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        if (students_vvu[i_vvu].name_vvu == searchName_vvu &&
            students_vvu[i_vvu].rollNo_vvu == searchRoll_vvu &&
            students_vvu[i_vvu].class_vvu == searchClass_vvu &&
            students_vvu[i_vvu].division_vvu == searchDiv_vvu) {
            
            cout << "\nStudent Found: " << students_vvu[i_vvu].name_vvu 
                 << " Roll No: " << students_vvu[i_vvu].rollNo_vvu 
                 << " Class: " << students_vvu[i_vvu].class_vvu 
                 << " Division: " << students_vvu[i_vvu].division_vvu << "\n";
            found_vvu = true;
            break;
        }
    }

    if (!found_vvu) {
        cout << "\nStudent not found.\n";
    }

    return 0;
}
```

## Example Output  
```
Enter total number of students: 3

Enter details of student 1:
Name: ABC
Roll Number: 10
Class (S.Y./T.Y./B.Tech): S.Y.
Division: X

Enter details of student 2:
Name: XYZ
Roll Number: 17
Class (S.Y./T.Y./B.Tech): S.Y.
Division: X

Enter details of student 3:
Name: PQR
Roll Number: 22
Class (S.Y./T.Y./B.Tech): B.Tech
Division: Y

Student Found: XYZ Roll No: 17 Class: S.Y. Division: X
```
