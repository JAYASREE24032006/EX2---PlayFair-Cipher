# EX2 - PLAYFAIR CIPHER
## AIM :
To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

## THEORY : 

The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre. The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. 
        To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do. If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping around to the left side of the row if a letter in the original pair was on the right side of the row).If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around to the top side of the column if a letter in the original pair was on the bottom side of the column).If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that lies on the same row as the first letter of the plaintext pair. To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).
        
## ALGORITHM :
### STEP 1 :
Design of PlayFair Cipher algorithnm

### STEP 2 :
Implementation using C or pyhton code

### STEP 3 :
Testing algorithm with different key values.

## THEORY : 

The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre. The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. 
        To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do. If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping around to the left side of the row if a letter in the original pair was on the right side of the row).If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around to the top side of the column if a letter in the original pair was on the bottom side of the column).If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that lies on the same row as the first letter of the plaintext pair. To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).
        
## PROGRAM :
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5
void generateKeyTable(char key[], int keyLength, char keyTable[SIZE][SIZE])
{
   int used[26] = {0};
   int i, j, k = 0;
   for (i = 0; i < keyLength; i++)
   {
       if (key[i] != 'j')
       {
           if (used[key[i] - 'a'] == 0) 
           {
               used[key[i] - 'a'] = 1;
               keyTable[k / SIZE][k % SIZE] = key[i];
               k++;
           }
       }
   }
   for (i = 0; i < 26; i++) 
   {
       if (i + 'a' != 'j' && used[i] == 0) 
       {
           keyTable[k / SIZE][k % SIZE] = i + 'a';
           k++;
       }
   }
}
void search(char keyTable[SIZE][SIZE], char a, char b, int *row1, int *col1, int *row2, int *col2)
{
   int i, j;
   for (i = 0; i < SIZE; i++) 
   {
       for (j = 0; j < SIZE; j++) 
       {
           if (keyTable[i][j] == a)
           {
               *row1 = i;
               *col1 = j;
           } 
           else if (keyTable[i][j] == b)
           {
               *row2 = i;
               *col2 = j;
           }
       }
   }
}
void encryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) 
{
   int row1, col1, row2, col2;
   search(keyTable, a, b, &row1, &col1, &row2, &col2);
   if (row1 == row2) 
   {
       *x = keyTable[row1][(col1 + 1) % SIZE];
       *y = keyTable[row2][(col2 + 1) % SIZE];
   }
   else if (col1 == col2)
   {
       *x = keyTable[(row1 + 1) % SIZE][col1];
       *y = keyTable[(row2 + 1) % SIZE][col2];
   } 
   else
   {
       *x = keyTable[row1][col2];
       *y = keyTable[row2][col1];
   }
}
void decryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y)
{
   int row1, col1, row2, col2;
   search(keyTable, a, b, &row1, &col1, &row2, &col2);
   if (row1 == row2)
   {
       *x = keyTable[row1][(col1 + SIZE - 1) % SIZE];
       *y = keyTable[row2][(col2 + SIZE - 1) % SIZE];
   } 
   else if (col1 == col2)
   {
       *x = keyTable[(row1 + SIZE - 1) % SIZE][col1];
       *y = keyTable[(row2 + SIZE - 1) % SIZE][col2];
   } 
   else 
   {
       *x = keyTable[row1][col2];
       *y = keyTable[row2][col1];
   }
}
void encrypt(char keyTable[SIZE][SIZE], char plainText[], char cipherText[]) 
{
   int i, j = 0;
   char a, b;
   for (i = 0; plainText[i] != '\0'; i += 2) 
   {
       a = plainText[i];
       if (plainText[i + 1] != '\0') 
       {
           b = plainText[i + 1];
       }
       else 
       {
           b = 'x';
       }
       if (a == b) 
       {
           b = 'x';
           i--;
       }
       encryptPair(keyTable, a, b, &cipherText[j], &cipherText[j + 1]);
       j += 2;
   }
   cipherText[j] = '\0';
}
void decrypt(char keyTable[SIZE][SIZE], char cipherText[], char plainText[]) 
{
   int i, j = 0;
   char a, b;
   for (i = 0; cipherText[i] != '\0'; i += 2)
   {
       a = cipherText[i];
       b = cipherText[i + 1];
       decryptPair(keyTable, a, b, &plainText[j], &plainText[j + 1]);
       j += 2;
   }
   plainText[j] = '\0';
}
int main() 
{
   char key[100], plainText[100], cipherText[100], decryptedText[100];
   char keyTable[SIZE][SIZE];
   int i, keyLength;
   printf("Enter the key: ");
   scanf("%s", key);
   for (i = 0; key[i] != '\0'; i++) 
   {
       key[i] = tolower(key[i]);
   }
   keyLength = strlen(key);
   generateKeyTable(key, keyLength, keyTable);
   printf("Enter the plaintext: ");
   scanf("%s", plainText);
   for (i = 0; plainText[i] != '\0'; i++)
   {
       plainText[i] = tolower(plainText[i]);
   }
   encrypt(keyTable, plainText, cipherText);
   printf("Encrypted Text: %s\n", cipherText);
   decrypt(keyTable, cipherText, decryptedText);
   printf("Decrypted Text: %s\n", decryptedText);
   return 0;
}



```
## OUTPUT:
![image](https://github.com/user-attachments/assets/2fc7cc5c-9df2-4eef-932f-233a854a6eb7)


## RESULT:
The program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique is executed successfully.
