# 🧩 CTF Challenge - hash-only-1
Category: Binary Exploitation

📁 Challenge Resources

- flaghasher (binary)
- PicoCTF Webshell

## 🕵️‍♂️ Description

Here is a binary that has enough privilege to read the content of the flag file but will only let you know its hash. 
If only it could just give you the actual content!

We can connect to a remote server in the web shell with the given connection:

<img width="406" height="227" alt="a7195fc0a103ef39d42297e03b308229-1" src="https://github.com/user-attachments/assets/aa18ef79-e88b-4fef-a724-c0c096e5fe53" />

## Running the Binary
Before diving into reverse engineering, I ran the binary within the connected server to observe its behavior:

<img width="252" height="76" alt="88f7564caad54bb3a2543a37b761bb66" src="https://github.com/user-attachments/assets/dc7b76b6-613b-427a-b427-03bcddb90562" />

From the output, we could see that the flag resides at /root/flag.txt, and the binary was computing its MD5 hash.
Next, I opened the binary in Ghidra to understand its logic.

## Outstanding Information

A key string stood out:

<img width="377" height="341" alt="be4ea5a3604f374e4e360eeec0b1e57a" src="https://github.com/user-attachments/assets/37506a46-1feb-446b-90e8-b8322b60eab9" />

Since the binary relies on md5sum, I realized we could exploit this by manipulating the PATH. 
By creating a custom md5sum binary (simply a copy of cat) in a directory we control and renaming it to md5sum, we can override the system command. 
Running the flaghasher binary afterward prints the flag directly.

<img width="263" height="63" alt="dd313aea8353659e3ff345988f6a221b" src="https://github.com/user-attachments/assets/1a38717c-7156-479b-9eb6-0b1f3fc5675b" />


## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 26th, 2025  
