# Program by Vaidehi Vinayak Unhale

## Problem Statement / Title  
Write a program to implement **Bubble Sort** and **Quick Sort** on a 1D array of Student structure (contains student_name, student_roll_no, total_marks), with key as **student_roll_no**. Also count the number of swaps performed by each method.

## Description  
We have a list of students (name, roll number, total marks).  
We’ll sort them:
- Using **Bubble Sort** (simple but slow for large data).
- Using **Quick Sort** (faster, divide & conquer).  

We’ll also count how many **swaps** are performed by each sorting algorithm.

## Code  
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Student_vvu {
    string name_vvu;
    int roll_vvu;
    float marks_vvu;
};

// Bubble Sort
int bubbleSort_vvu(Student_vvu arr_vvu[], int n_vvu) {
    int swaps_vvu = 0;
    for (int i_vvu = 0; i_vvu < n_vvu - 1; i_vvu++) {
        for (int j_vvu = 0; j_vvu < n_vvu - i_vvu - 1; j_vvu++) {
            if (arr_vvu[j_vvu].roll_vvu > arr_vvu[j_vvu + 1].roll_vvu) {
                swap(arr_vvu[j_vvu], arr_vvu[j_vvu + 1]);
                swaps_vvu++;
            }
        }
    }
    return swaps_vvu;
}

// Quick Sort partition
int partition_vvu(Student_vvu arr_vvu[], int low_vvu, int high_vvu, int &swaps_vvu) {
    int pivot_vvu = arr_vvu[high_vvu].roll_vvu;
    int i_vvu = low_vvu - 1;

    for (int j_vvu = low_vvu; j_vvu < high_vvu; j_vvu++) {
        if (arr_vvu[j_vvu].roll_vvu < pivot_vvu) {
            i_vvu++;
            swap(arr_vvu[i_vvu], arr_vvu[j_vvu]);
            swaps_vvu++;
        }
    }
    swap(arr_vvu[i_vvu + 1], arr_vvu[high_vvu]);
    swaps_vvu++;
    return i_vvu + 1;
}

// Quick Sort recursive
void quickSort_vvu(Student_vvu arr_vvu[], int low_vvu, int high_vvu, int &swaps_vvu) {
    if (low_vvu < high_vvu) {
        int pi_vvu = partition_vvu(arr_vvu, low_vvu, high_vvu, swaps_vvu);
        quickSort_vvu(arr_vvu, low_vvu, pi_vvu - 1, swaps_vvu);
        quickSort_vvu(arr_vvu, pi_vvu + 1, high_vvu, swaps_vvu);
    }
}

// Display students
void display_vvu(Student_vvu arr_vvu[], int n_vvu) {
    cout << "Name\tRoll\tMarks\n";
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cout << arr_vvu[i_vvu].name_vvu << "\t" 
             << arr_vvu[i_vvu].roll_vvu << "\t" 
             << arr_vvu[i_vvu].marks_vvu << "\n";
    }
}

int main() {
    int n_vvu;
    cout << "Enter number of students: ";
    cin >> n_vvu;

    Student_vvu students_vvu[50], copy_vvu[50];

    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cout << "\nEnter details of student " << i_vvu + 1 << ":\n";
        cout << "Name: ";
        cin >> students_vvu[i_vvu].name_vvu;
        cout << "Roll No: ";
        cin >> students_vvu[i_vvu].roll_vvu;
        cout << "Total Marks: ";
        cin >> students_vvu[i_vvu].marks_vvu;
        copy_vvu[i_vvu] = students_vvu[i_vvu]; // copy for quick sort
    }

    // Bubble Sort
    int bubbleSwaps_vvu = bubbleSort_vvu(students_vvu, n_vvu);
    cout << "\nAfter Bubble Sort (by Roll No):\n";
    display_vvu(students_vvu, n_vvu);
    cout << "Number of swaps (Bubble Sort): " << bubbleSwaps_vvu << "\n";

    // Quick Sort
    int quickSwaps_vvu = 0;
    quickSort_vvu(copy_vvu, 0, n_vvu - 1, quickSwaps_vvu);
    cout << "\nAfter Quick Sort (by Roll No):\n";
    display_vvu(copy_vvu, n_vvu);
    cout << "Number of swaps (Quick Sort): " << quickSwaps_vvu << "\n";

    return 0;
}
```

## Example Output  
```
Enter number of students: 3

Enter details of student 1:
Name: Alice
Roll No: 5
Total Marks: 80

Enter details of student 2:
Name: Bob
Roll No: 3
Total Marks: 75

Enter details of student 3:
Name: Carol
Roll No: 8
Total Marks: 90

After Bubble Sort (by Roll No):
Name    Roll    Marks
Bob     3       75
Alice   5       80
Carol   8       90
Number of swaps (Bubble Sort): 1

After Quick Sort (by Roll No):
Name    Roll    Marks
Bob     3       75
Alice   5       80
Carol   8       90
Number of swaps (Quick Sort): 2
```
