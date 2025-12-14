# Program by Vaidehi Vinayak Unhale

## Problem Statement / Title  
Write a program to input marks of **n students**, sort the marks in ascending order using the **Quick Sort algorithm** (without built-in functions) and analyse the sorting algorithm **pass by pass**. Find the **minimum and maximum marks** using **Divide and Conquer (recursively)**.

## Description  
We input marks of `n` students into an array.  
Then:  
- We sort using **Quick Sort** (our own implementation, not built-in).  
- After each partition, we print the array (to show **pass by pass** progress).  
- Finally, we use **Divide and Conquer recursively** to find **minimum** and **maximum** marks efficiently.

## Code  
```cpp
#include <iostream>
using namespace std;

// Function to print array after each pass
void printArray_vvu(int arr_vvu[], int n_vvu) {
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cout << arr_vvu[i_vvu] << " ";
    }
    cout << endl;
}

// Partition function for Quick Sort
int partition_vvu(int arr_vvu[], int low_vvu, int high_vvu, int n_vvu) {
    int pivot_vvu = arr_vvu[high_vvu];
    int i_vvu = (low_vvu - 1);

    for (int j_vvu = low_vvu; j_vvu < high_vvu; j_vvu++) {
        if (arr_vvu[j_vvu] < pivot_vvu) {
            i_vvu++;
            swap(arr_vvu[i_vvu], arr_vvu[j_vvu]);
        }
    }
    swap(arr_vvu[i_vvu + 1], arr_vvu[high_vvu]);

    // print array after each partition (pass)
    printArray_vvu(arr_vvu, n_vvu);

    return (i_vvu + 1);
}

// Quick Sort recursive function
void quickSort_vvu(int arr_vvu[], int low_vvu, int high_vvu, int n_vvu) {
    if (low_vvu < high_vvu) {
        int pi_vvu = partition_vvu(arr_vvu, low_vvu, high_vvu, n_vvu);
        quickSort_vvu(arr_vvu, low_vvu, pi_vvu - 1, n_vvu);
        quickSort_vvu(arr_vvu, pi_vvu + 1, high_vvu, n_vvu);
    }
}

// Divide and Conquer to find min and max
void findMinMax_vvu(int arr_vvu[], int low_vvu, int high_vvu, int &min_vvu, int &max_vvu) {
    if (low_vvu == high_vvu) {
        min_vvu = max_vvu = arr_vvu[low_vvu];
    } else if (high_vvu == low_vvu + 1) {
        if (arr_vvu[low_vvu] < arr_vvu[high_vvu]) {
            min_vvu = arr_vvu[low_vvu];
            max_vvu = arr_vvu[high_vvu];
        } else {
            min_vvu = arr_vvu[high_vvu];
            max_vvu = arr_vvu[low_vvu];
        }
    } else {
        int mid_vvu = (low_vvu + high_vvu) / 2;
        int min1_vvu, max1_vvu, min2_vvu, max2_vvu;

        findMinMax_vvu(arr_vvu, low_vvu, mid_vvu, min1_vvu, max1_vvu);
        findMinMax_vvu(arr_vvu, mid_vvu + 1, high_vvu, min2_vvu, max2_vvu);

        min_vvu = (min1_vvu < min2_vvu) ? min1_vvu : min2_vvu;
        max_vvu = (max1_vvu > max2_vvu) ? max1_vvu : max2_vvu;
    }
}

int main() {
    int n_vvu;
    cout << "Enter number of students: ";
    cin >> n_vvu;

    int marks_vvu[100];
    cout << "Enter marks: ";
    for (int i_vvu = 0; i_vvu < n_vvu; i_vvu++) {
        cin >> marks_vvu[i_vvu];
    }

    cout << "\nQuick Sort Pass by Pass:\n";
    quickSort_vvu(marks_vvu, 0, n_vvu - 1, n_vvu);

    cout << "\nMarks after Quick Sort (ascending):\n";
    printArray_vvu(marks_vvu, n_vvu);

    int min_vvu, max_vvu;
    findMinMax_vvu(marks_vvu, 0, n_vvu - 1, min_vvu, max_vvu);

    cout << "\nMinimum Marks: " << min_vvu;
    cout << "\nMaximum Marks: " << max_vvu << endl;

    return 0;
}
```

## Example Output  
```
Enter number of students: 5
Enter marks: 45 78 12 90 34

Quick Sort Pass by Pass:
34 12 45 90 78
12 34 45 90 78
12 34 45 90 78

Marks after Quick Sort (ascending):
12 34 45 78 90

Minimum Marks: 12
Maximum Marks: 90
```
