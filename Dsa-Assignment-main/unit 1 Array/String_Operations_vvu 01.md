# Implement Basic String Operations Without Using Built-in Functions

**Name:** Vaidehi Vinayak Unhale  

**Problem Statement / Title:**  
Implement basic string operations such as length calculation, copy, reverse, and concatenation using character single dimensional arrays without using built-in string library functions.  

**Description:**  
This program demonstrates how to perform common string operations manually using arrays in C.  
It includes functions for calculating string length, copying one string to another, reversing a string, and concatenating two strings.  
No built-in string functions are used — everything is implemented step by step.  

---

## Code

```c
#include <stdio.h>

// Function to calculate length
int stringLength_vvu(char str_vvu[]) {
    int i_vvu = 0;
    while (str_vvu[i_vvu] != '\0') {
        i_vvu++;
    }
    return i_vvu;
}

// Function to copy string
void stringCopy_vvu(char dest_vvu[], char src_vvu[]) {
    int i_vvu = 0;
    while (src_vvu[i_vvu] != '\0') {
        dest_vvu[i_vvu] = src_vvu[i_vvu];
        i_vvu++;
    }
    dest_vvu[i_vvu] = '\0';
}

// Function to reverse string
void stringReverse_vvu(char str_vvu[], char rev_vvu[]) {
    int len_vvu = stringLength_vvu(str_vvu);
    int i_vvu;
    for (i_vvu = 0; i_vvu < len_vvu; i_vvu++) {
        rev_vvu[i_vvu] = str_vvu[len_vvu - i_vvu - 1];
    }
    rev_vvu[i_vvu] = '\0';
}

// Function to concatenate two strings
void stringConcat_vvu(char str1_vvu[], char str2_vvu[], char result_vvu[]) {
    int i_vvu = 0, j_vvu = 0;

    while (str1_vvu[i_vvu] != '\0') {
        result_vvu[i_vvu] = str1_vvu[i_vvu];
        i_vvu++;
    }

    while (str2_vvu[j_vvu] != '\0') {
        result_vvu[i_vvu] = str2_vvu[j_vvu];
        i_vvu++;
        j_vvu++;
    }

    result_vvu[i_vvu] = '\0';
}

int main() {
    char str1_vvu[100], str2_vvu[100], copy_vvu[100], rev_vvu[100], concat_vvu[200];

    printf("Enter first string: ");
    scanf("%s", str1_vvu);

    printf("Enter second string: ");
    scanf("%s", str2_vvu);

    printf("\nLength of first string: %d\n", stringLength_vvu(str1_vvu));

    stringCopy_vvu(copy_vvu, str1_vvu);
    printf("Copied string: %s\n", copy_vvu);

    stringReverse_vvu(str1_vvu, rev_vvu);
    printf("Reversed string: %s\n", rev_vvu);

    stringConcat_vvu(str1_vvu, str2_vvu, concat_vvu);
    printf("Concatenated string: %s\n", concat_vvu);

    return 0;
}
```

---

## Sample Output

```
Enter first string: hello
Enter second string: world

Length of first string: 5
Copied string: hello
Reversed string: olleh
Concatenated string: helloworld
```
