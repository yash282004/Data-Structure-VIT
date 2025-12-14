# Program by Vaidehi Vinayak Unhale  

## Problem Statement / Title  
Write a program to arrange the list of employees as per the **average of their height and weight** by using **Merge Sort** and **Selection Sort**. Analyse their time complexities and conclude which algorithm will take less time to sort the list.

## Description (Easy Style)  
We have a list of employees, each with height and weight. We calculate the **average** of (height + weight) / 2 for each employee.  
We will:
- Sort employees based on this average using **Merge Sort**  
- Sort employees based on this average using **Selection Sort**  
- Compare time complexities:  
  - Merge Sort → **O(n log n)**  
  - Selection Sort → **O(n²)**  
Then conclude which is faster.

## Code  

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

struct Employee_vvu {
    string name_vvu;
    float height_vvu;
    float weight_vvu;
    float avg_vvu; // average of height and weight
};

// Function to compute averages
void computeAverage_vvu(vector<Employee_vvu> &emp_vvu) {
    for (auto &e_vvu : emp_vvu) {
        e_vvu.avg_vvu = (e_vvu.height_vvu + e_vvu.weight_vvu) / 2.0f;
    }
}

// Print employees
void printEmployees_vvu(const vector<Employee_vvu> &emp_vvu) {
    cout << "Name\tHeight\tWeight\tAverage\n";
    for (auto &e_vvu : emp_vvu) {
        cout << e_vvu.name_vvu << "\t" << e_vvu.height_vvu << "\t" << e_vvu.weight_vvu
             << "\t" << e_vvu.avg_vvu << "\n";
    }
}

// Merge Sort
void merge_vvu(vector<Employee_vvu> &arr_vvu, int l_vvu, int m_vvu, int r_vvu) {
    int n1_vvu = m_vvu - l_vvu + 1;
    int n2_vvu = r_vvu - m_vvu;

    vector<Employee_vvu> L_vvu(n1_vvu), R_vvu(n2_vvu);

    for (int i_vvu = 0; i_vvu < n1_vvu; i_vvu++)
        L_vvu[i_vvu] = arr_vvu[l_vvu + i_vvu];
    for (int j_vvu = 0; j_vvu < n2_vvu; j_vvu++)
        R_vvu[j_vvu] = arr_vvu[m_vvu + 1 + j_vvu];

    int i_vvu = 0, j_vvu = 0, k_vvu = l_vvu;
    while (i_vvu < n1_vvu && j_vvu < n2_vvu) {
        if (L_vvu[i_vvu].avg_vvu <= R_vvu[j_vvu].avg_vvu) {
            arr_vvu[k_vvu] = L_vvu[i_vvu];
            i_vvu++;
        } else {
            arr_vvu[k_vvu] = R_vvu[j_vvu];
            j_vvu++;
        }
        k_vvu++;
    }

    while (i_vvu < n1_vvu) {
        arr_vvu[k_vvu] = L_vvu[i_vvu];
        i_vvu++;
        k_vvu++;
    }

    while (j_vvu < n2_vvu) {
        arr_vvu[k_vvu] = R_vvu[j_vvu];
        j_vvu++;
        k_vvu++;
    }
}

void mergeSort_vvu(vector<Employee_vvu> &arr_vvu, int l_vvu, int r_vvu) {
    if (l_vvu < r_vvu) {
        int m_vvu = l_vvu + (r_vvu - l_vvu) / 2;
        mergeSort_vvu(arr_vvu, l_vvu, m_vvu);
        mergeSort_vvu(arr_vvu, m_vvu + 1, r_vvu);
        merge_vvu(arr_vvu, l_vvu, m_vvu, r_vvu);
    }
}

// Selection Sort
void selectionSort_vvu(vector<Employee_vvu> &arr_vvu) {
    int n_vvu = arr_vvu.size();
    for (int i_vvu = 0; i_vvu < n_vvu - 1; i_vvu++) {
        int minIndex_vvu = i_vvu;
        for (int j_vvu = i_vvu + 1; j_vvu < n_vvu; j_vvu++) {
            if (arr_vvu[j_vvu].avg_vvu < arr_vvu[minIndex_vvu].avg_vvu) {
                minIndex_vvu = j_vvu;
            }
        }
        if (minIndex_vvu != i_vvu) {
            swap(arr_vvu[i_vvu], arr_vvu[minIndex_vvu]);
        }

        cout << "After pass " << i_vvu + 1 << ":\n";
        printEmployees_vvu(arr_vvu);
        cout << "\n";
    }
}

int main() {
    int n_vvu;
    cout << "Enter number of employees: ";
    cin >> n_vvu;

    vector<Employee_vvu> employees_vvu(n_vvu);
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cout << "\nEnter name: ";
        cin >> employees_vvu[i_vvu].name_vvu;
        cout << "Enter height: ";
        cin >> employees_vvu[i_vvu].height_vvu;
        cout << "Enter weight: ";
        cin >> employees_vvu[i_vvu].weight_vvu;
    }

    computeAverage_vvu(employees_vvu);

    cout << "\nOriginal List:\n";
    printEmployees_vvu(employees_vvu);

    vector<Employee_vvu> employeesMerge_vvu = employees_vvu;
    vector<Employee_vvu> employeesSelect_vvu = employees_vvu;

    // Merge Sort
    mergeSort_vvu(employeesMerge_vvu, 0, n_vvu - 1);
    cout << "\nAfter Merge Sort (by average):\n";
    printEmployees_vvu(employeesMerge_vvu);

    // Selection Sort with pass-by-pass
    cout << "\nSelection Sort (pass by pass):\n";
    selectionSort_vvu(employeesSelect_vvu);

    cout << "\nAfter Selection Sort (by average):\n";
    printEmployees_vvu(employeesSelect_vvu);

    cout << "\nTime Complexity:\n";
    cout << "Merge Sort: O(n log n)\n";
    cout << "Selection Sort: O(n^2)\n";
    cout << "Conclusion: Merge Sort is faster for larger lists.\n";

    return 0;
}
```

## Example Output  
```
Enter number of employees: 3
Enter name: Vaidehi
Enter height: 170
Enter weight: 65

Enter name: Purvi
Enter height: 160
Enter weight: 70

Enter name: Sushant
Enter height: 180
Enter weight: 80

Original List:
Name    Height  Weight  Average
Vaidehi 170     65      117.5
Purvi   160     70      115
Sushant 180     80      130

After Merge Sort (by average):
Name    Height  Weight  Average
Purvi   160     70      115
Vaidehi 170     65      117.5
Sushant 180     80      130

Selection Sort (pass by pass):
After pass 1:
Name    Height  Weight  Average
Purvi   160     70      115
Vaidehi 170     65      117.5
Sushant 180     80      130

After pass 2:
Name    Height  Weight  Average
Purvi   160     70      115
Vaidehi 170     65      117.5
Sushant 180     80      130

After Selection Sort (by average):
Name    Height  Weight  Average
Purvi   160     70      115
Vaidehi 170     65      117.5
Sushant 180     80      130

Time Complexity:
Merge Sort: O(n log n)
Selection Sort: O(n^2)
Conclusion: Merge Sort is faster for larger lists.
```
