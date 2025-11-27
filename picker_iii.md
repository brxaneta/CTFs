# 🧩 CTF Challenge - Picker III
Category: Reverse Engineering

📁 Challenge Resources

- picker-III.py
- PicoCTF Webshell

## 🕵️‍♂️ Description
Can you figure out how this program works to get the flag?

<img width="288" height="74" alt="6c991f95130f63f904ff555260377155" src="https://github.com/user-attachments/assets/5d01e1b4-8d76-49e2-b1c7-49b18dd2b49c" />

## Inspect the Code
Inside the write_variable() function, the vulnerability lies in the following line:
```python
exec('global ' + var_name + '; ' + var_name + ' = ' + value)
```
Because exec() is used together with global, the function can create or overwrite global variables based on user‑controlled input. 
This means an attacker can redefine names used elsewhere in the program - including other functions.

By supplying the value "win" to write_variable(), we overwrite the targeted global variable with a reference to the win() function. Later, when read_variable() retrieves and executes that variable, it ends up calling win() directly, which prints the contents of flag.txt.

<img width="419" height="164" alt="2a6b7363da828bd93f618bb9d4b1e90e" src="https://github.com/user-attachments/assets/3226c749-e7d0-4635-92c5-52fc0bad32be" />

Then, I simply pasted the hex into CyberChef and got the flag!

## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 26th, 2025  
