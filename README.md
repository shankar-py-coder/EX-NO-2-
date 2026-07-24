## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a Python program to implement the Playfair Substitution technique.

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
```
def generate_key_table(key):
    key = key.lower().replace('j', 'i')
    table = []
    used = set()

    for ch in key:
        if ch.isalpha() and ch not in used:
            used.add(ch)
            table.append(ch)

    for ch in "abcdefghiklmnopqrstuvwxyz":
        if ch not in used:
            used.add(ch)
            table.append(ch)

    return [table[i:i+5] for i in range(0, 25, 5)]

def prepare_text(text):
    text = text.lower().replace(" ", "").replace("j", "i")
    result = ""
    i = 0
    while i < len(text):
        a = text[i]
        if i + 1 < len(text):
            b = text[i + 1]
            if a == b:
                result += a + "x"
                i += 1
            else:
                result += a + b
                i += 2
        else:
            result += a + "x"
            i += 1
    return result

def encrypt(text, table):
    pos = {}
    for i in range(5):
        for j in range(5):
            pos[table[i][j]] = (i, j)

    cipher = ""

    for i in range(0, len(text), 2):
        a, b = text[i], text[i + 1]
        r1, c1 = pos[a]
        r2, c2 = pos[b]

        if r1 == r2:
            cipher += table[r1][(c1 + 1) % 5]
            cipher += table[r2][(c2 + 1) % 5]
        elif c1 == c2:
            cipher += table[(r1 + 1) % 5][c1]
            cipher += table[(r2 + 1) % 5][c2]
        else:
            cipher += table[r1][c2]
            cipher += table[r2][c1]

    return cipher

key = input("Enter the keyword: ")
plaintext = input("Enter the plaintext: ")

key_table = generate_key_table(key)
prepared = prepare_text(plaintext)
cipher = encrypt(prepared, key_table)

print("\nKey Matrix:")
for row in key_table:
    print(" ".join(row))

print("\nCipher Text:", cipher)
```




Output:
<img width="1697" height="752" alt="image" src="https://github.com/user-attachments/assets/84a125cf-a288-42fa-b2ed-52935c437050" />
