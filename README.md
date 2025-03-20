## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:

import re

def generate_matrix(key):
   
    key = "".join(dict.fromkeys(key.upper().replace("J", "I") + "ABCDEFGHIKLMNOPQRSTUVWXYZ"))
    return [list(key[i:i+5]) for i in range(0, 25, 5)]

def find_position(matrix, letter):
    
    for r, row in enumerate(matrix):
        if letter in row:
            return r, row.index(letter)
    return None

def playfair_cipher(text, key, encrypt=True):
   
    text = re.sub(r'[^A-Z]', '', text.upper().replace("J", "I"))
    if len(text) % 2: text += 'X'
    matrix = generate_matrix(key)
    result, shift = "", 1 if encrypt else -1
    
    for i in range(0, len(text), 2):
        a, b = text[i], text[i+1]
        row_a, col_a = find_position(matrix, a)
        row_b, col_b = find_position(matrix, b)
        
        if row_a == row_b:
            result += matrix[row_a][(col_a + shift) % 5] + matrix[row_b][(col_b + shift) % 5]
        elif col_a == col_b:
            result += matrix[(row_a + shift) % 5][col_a] + matrix[(row_b + shift) % 5][col_b]
        else:
            result += matrix[row_a][col_b] + matrix[row_b][col_a]
    
    return result



key = input("Enter a key:")

plaintext = input("Enter a text:")

encrypted = playfair_cipher(plaintext, key, True)

decrypted = playfair_cipher(encrypted, key, False)


print("Encrypted:", encrypted)

print("Decrypted:", decrypted)






Output:



![Screenshot 2025-03-20 092944](https://github.com/user-attachments/assets/62ee04bf-bcf0-4c58-abc7-634d67cf6a33)

