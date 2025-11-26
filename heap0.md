# 🧩 CTF Challenge - heap0
Category: Binary Exploitation

📁 Challenge Resources

- chall
- chall.c
- Pico CTF Web Shell


## 🕵️‍♂️ Description

Are overflows just a stack concern?

<img width="420" height="239" alt="2e6ffeb68d6af3a9caf7e7e547c273e6" src="https://github.com/user-attachments/assets/b81fafa4-14dc-4cbd-b5cf-a1e8bfb57485" />

## Inspect the Source Code

After reviewing the source code, I found that the program places two variables - input_data and safe_var - on the heap. 
input_data starts with the value "pico", while safe_var is initialized as "bico". 
The objective of the challenge is to overwrite the contents of safe_var; doing so causes the program to reveal the flag stored in a file.

<img width="377" height="241" alt="b110399269dc2288573e93efd01a2b8b" src="https://github.com/user-attachments/assets/ee634997-2e01-4190-889c-c73ffabeeaa2" />

## Figuring it Out
To figure out the exact number of bytes needed to overwrite safe_var, I used the program’s print heap feature to inspect the memory layout. It displayed the following heap entries:

<img width="175" height="100" alt="6511c78824b767be6e13985101910547" src="https://github.com/user-attachments/assets/e1e35a7f-09ea-44ab-aa4e-2f6a0e5ad3fb" />

Both values are heap addresses in hexadecimal. Calculating the distance between them gives us the offset we need:

- input_data is at 0x63c3882552b0
- safe_var is at 0x63c3882552d0

Subtracting the two addresses:
0x63c3882552d0 - 0x63c3882552b0 = 0x20

0x20 bytes equals 32 bytes, meaning that once we write 33 or more characters, the overflow spills into safe_var and overwrites the "bico" value. 
At that point, the program’s check_win() function succeeds and prints the flag.
Here, I used 33 'A's to trigger it.

<img width="415" height="251" alt="64cb134e01768952a8d1142b13529e5f" src="https://github.com/user-attachments/assets/791f3fd0-740a-4d4d-850e-ba56fb5c2299" />

## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 25th, 2025  
