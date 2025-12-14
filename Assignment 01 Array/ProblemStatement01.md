# Implement Basic String Operations Without Using Built-in Functions

**Name:** Yash Santosh Khandagale  

**Problem Statement / Title:**  
Implement basic string operations such as length calculation, copy, reverse, and concatenation using character single dimensional arrays without using built-in string library functions.  

**Description:**  
This program demonstrates how to perform common string operations manually using arrays in C.  
It includes functions for calculating string length, copying one string to another, reversing a string, and concatenating two strings.  
No built-in string functions are used — everything is implemented step by step.  

---

## Code

```c
/*
    Implement basic string operations such as length calculation, 
    copy, reverse, and concatenation using character single dimensional
    arrays without using built-in string library functions.	
*/
#include<stdio.h>

int menu();
int calculateLength(char str_ysk[]);
void copyString(char src_ysk[], char dest_ysk[]);
void reverseString(char src_ysk[], char dest_ysk[]);
void concatString(char str1_ysk[], char str2_ysk[], char result_ysk[]);

int main(void){
    int choice_ysk=0;
    char str1_ysk[100], str2_ysk[100], result_ysk[200];

    do{
        choice_ysk=menu();
        switch(choice_ysk){
            case 1: 
                printf("Enter a string :\n");
                scanf(" %s", str1_ysk);   
                break;
                    
            case 2: {
                int len_ysk=calculateLength(str1_ysk);
                printf("Length of String : %d \n",len_ysk);
                break;
            }
                    
            case 3: 
                copyString(str1_ysk,result_ysk);
                printf("Copied string = %s\n", result_ysk);
                break;

            case 4: 
                reverseString(str1_ysk,result_ysk);
                printf("Reversed string = %s\n", result_ysk);
                break;

            case 5: 
                printf("Enter another string to concatenate :\n");
                scanf(" %s", str2_ysk);
                concatString(str1_ysk,str2_ysk,result_ysk);
                printf("Concatenated string = %s\n", result_ysk);
                break;

            case 6: 
                printf("Thank you for using our software\n");
                break;

            default:
                printf("Invalid choice! Try again.\n");
        }
    }while(choice_ysk!=6);
    return 0;
}

int menu(){
    int choice_ysk;
    printf("\n********  MENU ********\n");
    printf("1. Enter String :\n");
    printf("2. Calculate String Length :\n");
    printf("3. Copy String :\n");
    printf("4. Reverse a String :\n");
    printf("5. Concatenation of String :\n");
    printf("6. Exit\n");
    scanf("%d",&choice_ysk);
    return choice_ysk;
}

int calculateLength(char str_ysk[]){
    int length_ysk=0;
    while(str_ysk[length_ysk]!='\0'){   
        length_ysk++;
    }
    return length_ysk;
}

void copyString(char src_ysk[], char dest_ysk[]) {
    int i_ysk=0;
    while(src_ysk[i_ysk]!='\0') {
        dest_ysk[i_ysk]=src_ysk[i_ysk];
        i_ysk++;
    }
    dest_ysk[i_ysk]='\0';
}

void reverseString(char src_ysk[], char dest_ysk[]) {
    int len_ysk = calculateLength(src_ysk);
    int i_ysk;
    for(i_ysk=0; i_ysk<len_ysk; i_ysk++){
        dest_ysk[i_ysk]=src_ysk[len_ysk-1-i_ysk];
    }
    dest_ysk[len_ysk]='\0';
}

void concatString(char str1_ysk[], char str2_ysk[], char result_ysk[]) {
    int i_ysk=0, j_ysk=0;
    while(str1_ysk[i_ysk]!='\0'){
        result_ysk[i_ysk]=str1_ysk[i_ysk];
        i_ysk++;
    }
    while(str2_ysk[j_ysk]!='\0'){
        result_ysk[i_ysk]=str2_ysk[j_ysk];
        i_ysk++;
        j_ysk++;
    }
    result_ysk[i_ysk]='\0';
}
```

---

## Sample Output

```
********  MENU ********
1. Enter String :
2. Calculate String Length :
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
1
Enter a string :
Yash

********  MENU ********
1. Enter String :
2. Calculate String Length :
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
2
Length of String : 4

********  MENU ********
1. Enter String :
2. Calculate String Length :
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
3
Copied string = Yash

********  MENU ********
1. Enter String :
2. Calculate String Length :
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
4
Reversed string = hsaY

********  MENU ********
1. Enter String :
2. Calculate String Length :
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
5
Enter another string to concatenate :
.Khandagale
Concatenated string = Yash.Khandagale

********  MENU ********
1. Enter String :
2. Calculate String Length :
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
5
Enter another string to concatenate :
.Khandagale
Concatenated string = Yash.Khandagale

********  MENU ********
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
5
Enter another string to concatenate :
.Khandagale
Concatenated string = Yash.Khandagale

********  MENU ********
4. Reverse a String :
5. Concatenation of String :
6. Exit
5
Enter another string to concatenate :
.Khandagale
Concatenated string = Yash.Khandagale

********  MENU ********
1. Enter String :
5. Concatenation of String :
6. Exit
5
Enter another string to concatenate :
.Khandagale
Concatenated string = Yash.Khandagale

********  MENU ********
1. Enter String :
Enter another string to concatenate :
.Khandagale
Concatenated string = Yash.Khandagale

********  MENU ********
1. Enter String :
Concatenated string = Yash.Khandagale

********  MENU ********
1. Enter String :
1. Enter String :
2. Calculate String Length :
3. Copy String :
4. Reverse a String :
5. Concatenation of String :
6. Exit
6
Thank you for using our software
```
