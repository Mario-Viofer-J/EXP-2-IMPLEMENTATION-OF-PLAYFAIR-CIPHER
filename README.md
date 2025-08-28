# EXP : 2 IMPLEMENTATION OF PLAY FAIR CIPHER

### NAME: MARIO VIOFER J
### REG NO: 212223100032
### DATE: 28.08.2025
 
 ## AIM :
 To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.
 
 
 ## ALGORITHM:
 1.	To perform Encryption :
 •	Preprocess the key:
 a.	Convert the key to lowercase.
 b.	Remove spaces and duplicate letters.
 c.	Replace 'j' with 'i'.
 2.	Generate the 5×5 key matrix (key table):
 a.	Fill in the unique characters of the key.
 b.	Fill the remaining cells with unused letters of the alphabet (except 'j').
 3.	Preprocess the plaintext:
 a.	Convert to lowercase.
 b.	Remove spaces.
 c.	Replace 'j' with 'i'.
 d.	Break into digraphs (pairs of two letters).
 If a pair has two same letters, insert 'x' between them.
 If there's a single letter left at the end, add a padding letter (like 'z').
 4.	Encrypt each digraph using these rules:
 a.	Same row: Replace each letter with the letter to its right (wrap to start if needed).
 b.	Same column: Replace each letter with the letter below it (wrap to top if needed).
 c.	Different row and column: Swap the column positions of the two letters.
 5.	Display the encrypted ciphertext.
 6.	To perform Decryption:
 •	Preprocess the ciphertext like the encryption step.
 •	Decrypt each digraph using these rules:
 a.	Same row: Replace each letter with the letter to its left (wrap to end if needed).
 
 b.	Same column: Replace each letter with the letter above it (wrap to bottom if needed).
 
 c.	Different row and column: Swap the column positions of the two letters.
 
 7.	Display the decrypted plaintext.
 
 ## PROGRAM :
 ```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateKeyMatrix(char key[], char matrix[SIZE][SIZE]) {
    int alpha[26] = {0};
    int i, j, k = 0;
    char current;

    for (i = 0; key[i] != '\0'; i++) {
        current = toupper(key[i]);
        if (current == 'J') current = 'I';
        if (current < 'A' || current > 'Z' || alpha[current - 'A'])
            continue;
        alpha[current - 'A'] = 1;
        key[k++] = current;
    }
    key[k] = '\0';

    i = 0; k = 0;
    for (int row = 0; row < SIZE; row++) {
        for (int col = 0; col < SIZE; col++) {
            if (i < strlen(key)) {
                matrix[row][col] = key[i++];
            } else {
                for (char ch = 'A'; ch <= 'Z'; ch++) {
                    if (ch == 'J' || alpha[ch - 'A'])
                        continue;
                    matrix[row][col] = ch;
                    alpha[ch - 'A'] = 1;
                    break;
                }
            }
        }
    }
}

void findPosition(char matrix[SIZE][SIZE], char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I';  
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void processDigraph(char a, char b, char matrix[SIZE][SIZE], char *resA, char *resB, int encrypt) {
    int row1, col1, row2, col2;
    findPosition(matrix, a, &row1, &col1);
    findPosition(matrix, b, &row2, &col2);

    if (row1 == row2) {  
        *resA = matrix[row1][(col1 + (encrypt ? 1 : SIZE - 1)) % SIZE];
        *resB = matrix[row2][(col2 + (encrypt ? 1 : SIZE - 1)) % SIZE];
    } else if (col1 == col2) {  
        *resA = matrix[(row1 + (encrypt ? 1 : SIZE - 1)) % SIZE][col1];
        *resB = matrix[(row2 + (encrypt ? 1 : SIZE - 1)) % SIZE][col2];
    } else {  // Rectangle swap
        *resA = matrix[row1][col2];
        *resB = matrix[row2][col1];
    }
}

void preprocessText(char *text) {
    char temp[100] = {0};
    int k = 0;

    for (int i = 0; text[i]; i++) {
        if (isalpha(text[i])) {
            temp[k++] = toupper(text[i] == 'J' ? 'I' : text[i]);
        }
    }
    temp[k] = '\0';
    strcpy(text, temp);
}

void encryptDecryptText(char *text, char matrix[SIZE][SIZE], int encrypt) {
    preprocessText(text);
    char result[100] = {0};
    int len = strlen(text), k = 0;

    for (int i = 0; i < len; i += 2) {
        char a = text[i];
        char b = (i + 1 < len) ? text[i + 1] : 'X';

        if (a == b) {
            b = 'X';  
            i--;      
        }

        char resA, resB;
        processDigraph(a, b, matrix, &resA, &resB, encrypt);
        result[k++] = resA;
        result[k++] = resB;
    }
    result[k] = '\0';
    strcpy(text, result);
}

void printMatrix(char matrix[SIZE][SIZE]) {
    printf("KEY MATRIX:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    char key[100], text[100], matrix[SIZE][SIZE];
    
    printf("ENTER THE KEY: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';

    printf("ENTER TEXT TO ENCRYPT: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = '\0';

    generateKeyMatrix(key, matrix);
    printMatrix(matrix);

    encryptDecryptText(text, matrix, 1);  
    printf("ENCRYPTED TEXT: %s\n", text);

    encryptDecryptText(text, matrix, 0);  
    printf("DECRYPTED TEXT: %s\n", text);

    return 0;
}

 ```
 
 ## OUTPUT :
  <img width="525" height="461" alt="Screenshot 2025-08-28 141838" src="https://github.com/user-attachments/assets/231563c0-eed1-4f7c-b462-ed4ab5ad2eb6" />

 ## RESULT:
The program is executed successfully.
