# 🧩 CTF Challenge - Picker IV
Category: Binary Exploitation

📁 Challenge Resources

- picker-iv
- picker-iv.c
- PicoCTF Webshell 


## 🕵️‍♂️ Description

Can you figure out how this program works to get the flag?

## Inspecting the binary
Looking at the binary’s source, it’s clear that the win function is responsible for printing the flag. 

<img width="191" height="183" alt="939174cdc58cdaf74e69cc032efa235d" src="https://github.com/user-attachments/assets/e46122b0-6f0f-4f5c-8b0d-386a023da9c0" />

The program also prompts the user with:
“Enter the address in hex to jump to.”

<img width="293" height="29" alt="13e7f837533c73de35bec842344b22c3" src="https://github.com/user-attachments/assets/a8e91947-84e9-4482-b2ef-b8fc3b0508f9" />

## Time to dig
This means that if we can determine the address of win and provide it as input, the binary will jump directly to that function and reveal the flag.
To locate the address of win, we can use gdb. 
```gdb
gdb picker-IV
```
After loading the binary, the info functions command lists all function symbols along with their memory addresses.

<img width="198" height="265" alt="ff7dcae099171d8c39015f35b597b5f6" src="https://github.com/user-attachments/assets/98530ef5-beb6-499e-975d-31e6433e1c51" />

The win function is at address 0x40129e. When we pass it to the binary... voila! We got the flag!

<img width="303" height="49" alt="cc19c2905802d9b774d39616a426e379" src="https://github.com/user-attachments/assets/707e8125-a6d0-4afe-a271-7b1ac419de71" />

## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 26th, 2025  
