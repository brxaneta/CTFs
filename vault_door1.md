# 🧩 CTF Challenge - Vault Door 1
Category: Reverse Engineering

📁 Challenge Resources

- VaultDoor1.java

## 🕵️‍♂️ Description

This vault uses some complicated arrays! I hope you can make sense of it, special agent.

## Inspecting the Code
```java
import java.util.*;

class VaultDoor1 {
    public static void main(String args[]) {
        VaultDoor1 vaultDoor = new VaultDoor1();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
	String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
	}
    }

    // I came up with a more secure way to check the password without putting
    // the password itself in the source code. I think this is going to be
    // UNHACKABLE!! I hope Dr. Evil agrees...
    //
    // -Minion #8728
    public boolean checkPassword(String password) {
        return password.length() == 32 &&
               password.charAt(0)  == 'd' &&
               password.charAt(29) == '5' &&
               password.charAt(4)  == 'r' &&
               password.charAt(2)  == '5' &&
               password.charAt(23) == 'r' &&
               password.charAt(3)  == 'c' &&
               password.charAt(17) == '4' &&
               password.charAt(1)  == '3' &&
               password.charAt(7)  == 'b' &&
               password.charAt(10) == '_' &&
               password.charAt(5)  == '4' &&
               password.charAt(9)  == '3' &&
               password.charAt(11) == 't' &&
               password.charAt(15) == 'c' &&
               password.charAt(8)  == 'l' &&
               password.charAt(12) == 'H' &&
               password.charAt(20) == 'c' &&
               password.charAt(14) == '_' &&
               password.charAt(6)  == 'm' &&
               password.charAt(24) == '5' &&
               password.charAt(18) == 'r' &&
               password.charAt(13) == '3' &&
               password.charAt(19) == '4' &&
               password.charAt(21) == 'T' &&
               password.charAt(16) == 'H' &&
               password.charAt(27) == '5' &&
               password.charAt(30) == '6' &&
               password.charAt(25) == '_' &&
               password.charAt(22) == '3' &&
               password.charAt(28) == '2' &&
               password.charAt(26) == 'd' &&
               password.charAt(31) == '5';
    }
}
```
The program expects input following the picoCTF{} format:
```Java
String input = userInput.substring("picoCTF{".length(), userInput.length()-1);
```
This strips off "picoCTF{" at the start and the closing } at the end.

## Analyzing the Password Check
The core of the binary is the checkPassword() function. It contains a long list of conditions:
```java
password.charAt(0)  == 'd'
password.charAt(1)  == '3'
password.charAt(2)  == '5'
...
password.charAt(31) == '5'
```
This acts like a map of:
```index -> required character```
Because the function explicitly defines every character needed, we don't need to manually reorder the characters - we can reconstruct the password using a script.

## Automatically Rebuilding the Password
To avoid human error and make the process efficient, we can write a short Python script that builds the password from the index rules.
```python
password = ['?'] * 32

rules = {
    0: 'd', 29: '5', 4: 'r', 2: '5', 23: 'r',
    3: 'c', 17: '4', 1: '3', 7: 'b', 10: '_',
    5: '4', 9: '3', 11: 't', 15: 'c', 8: 'l',
    12: 'H', 20: 'c', 14: '_', 6: 'm', 24: '5',
    18: 'r', 13: '3', 19: '4', 21: 'T', 16: 'H',
    27: '5', 30: '6', 25: '_', 22: '3', 28: '2',
    26: 'd', 31: '5'
}

for index, char in rules.items():
    password[index] = char

print("".join(password))
```

And there it is! Flag obtained!

<img width="494" height="33" alt="b76283feddee726a90e008744d2e194e" src="https://github.com/user-attachments/assets/828f62d4-a5ed-4bb2-957b-0c674ac8ce37" />


## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 26th, 2025  
