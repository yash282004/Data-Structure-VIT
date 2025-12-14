# Program by Vaidehi Vinayak Unhale

## Problem Statement / Title
Write a program using **Bubble Sort** algorithm, assign the roll nos. to the students of your class as per their previous year's result — i.e., topper will be roll no. 1 — and analyse the sorting algorithm **pass by pass**.

## Description (GeeksforGeeks style — simple & easy)
We have a list of students and their previous-year marks. We want to give roll numbers so that the student with highest marks gets roll number 1, next highest gets roll number 2, and so on.  
We'll use **Bubble Sort** to sort students in **descending order** by marks (so topper comes first). After each pass of the Bubble Sort, we'll print the current order of students to analyse how the algorithm progresses. After sorting, we assign roll numbers (1..n) according to the sorted order.

This helps you understand how Bubble Sort moves the larger elements toward the front (here: higher marks to the front) and lets you study how many swaps happen in each pass and when the array becomes sorted.

## Code
```cpp
#include <iostream>
#include <string>
#include <limits>
using namespace std;

struct Student_vvu {
    string name_vvu;
    int roll_vvu;
    int marks_vvu;
};

// Print students (Name, Marks, Roll)
void printStudents_vvu(Student_vvu arr_vvu[], int n_vvu) {
    cout << "Name\tMarks\tRoll\n";
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cout << arr_vvu[i_vvu].name_vvu << "\t"
             << arr_vvu[i_vvu].marks_vvu << "\t"
             << arr_vvu[i_vvu].roll_vvu << "\n";
    }
}

// Bubble sort in DESCENDING order by marks, analyze pass-by-pass, then assign rolls
int bubbleSortAssign_vvu(Student_vvu arr_vvu[], int n_vvu) {
    int totalSwaps_vvu = 0;
    for (int pass_vvu = 0; pass_vvu < n_vvu - 1; pass_vvu++) {
        int swapsThisPass_vvu = 0;
        for (int j_vvu = 0; j_vvu < n_vvu - pass_vvu - 1; j_vvu++) {
            // For topper -> roll 1 we want highest marks first => descending order
            if (arr_vvu[j_vvu].marks_vvu < arr_vvu[j_vvu + 1].marks_vvu) {
                swap(arr_vvu[j_vvu], arr_vvu[j_vvu + 1]);
                swapsThisPass_vvu++;
                totalSwaps_vvu++;
            }
        }
        cout << "After pass " << pass_vvu + 1 << ":\n";
        printStudents_vvu(arr_vvu, n_vvu);
        cout << "Swaps in this pass: " << swapsThisPass_vvu << "\n\n";
        // If no swaps occurred, the array is already sorted
        if (swapsThisPass_vvu == 0) break;
    }
    // Assign roll numbers: topper (index 0) -> roll 1
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        arr_vvu[i_vvu].roll_vvu = i_vvu + 1;
    }
    return totalSwaps_vvu;
}

int main() {
    int n_vvu;
    cout << "Enter number of students: ";
    cin >> n_vvu;
    cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // clear newline

    Student_vvu students_vvu[100];

    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cout << "\nEnter details of student " << i_vvu + 1 << ":\n";
        cout << "Name: ";
        getline(cin, students_vvu[i_vvu].name_vvu);
        cout << "Previous year marks: ";
        cin >> students_vvu[i_vvu].marks_vvu;
        students_vvu[i_vvu].roll_vvu = 0; // initialize
        cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // clear newline for next getline
    }

    cout << "\nInitial order:\n";
    printStudents_vvu(students_vvu, n_vvu);

    cout << "\nBubble Sort (pass by pass) — sorting by marks (highest first):\n\n";
    int totalSwaps_vvu = bubbleSortAssign_vvu(students_vvu, n_vvu);

    cout << "Final order after sorting and roll assignment:\n";
    printStudents_vvu(students_vvu, n_vvu);

    cout << "\nTotal number of swaps performed: " << totalSwaps_vvu << "\n";

    return 0;
}
```

## Example Output
```
Enter number of students: 5

Enter details of student 1:
Name: Alice Johnson
Previous year marks: 85

Enter details of student 2:
Name: Bob Kumar
Previous year marks: 92

Enter details of student 3:
Name: Carol Mehta
Previous year marks: 78

Enter details of student 4:
Name: Dave Singh
Previous year marks: 92

Enter details of student 5:
Name: Eve Patel
Previous year marks: 65

Initial order:
Name    Marks   Roll
Alice Johnson  85      0
Bob Kumar      92      0
Carol Mehta    78      0
Dave Singh     92      0
Eve Patel      65      0

Bubble Sort (pass by pass) — sorting by marks (highest first):

After pass 1:
Name    Marks   Roll
Bob Kumar      92      0
Alice Johnson  85      0
Dave Singh     92      0
Carol Mehta    78      0
Eve Patel      65      0
Swaps in this pass: 2

After pass 2:
Name    Marks   Roll
Bob Kumar      92      0
Dave Singh     92      0
Alice Johnson  85      0
Carol Mehta    78      0
Eve Patel      65      0
Swaps in this pass: 1

After pass 3:
Name    Marks   Roll
Bob Kumar      92      0
Dave Singh     92      0
Alice Johnson  85      0
Carol Mehta    78      0
Eve Patel      65      0
Swaps in this pass: 0

Final order after sorting and roll assignment:
Name    Marks   Roll
Bob Kumar      92      1
Dave Singh     92      2
Alice Johnson  85      3
Carol Mehta    78      4
Eve Patel      65      5

Total number of swaps performed: 3
```
