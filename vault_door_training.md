# 🧩 CTF Challenge - vault_door_training
📁 Challenge Files

- VaultDoorTraining.java


## 🕵️‍♂️ Description

Your mission is to enter Dr. Evil's laboratory and retrieve the blueprints for his Doomsday Project. The laboratory is protected by a series of locked vault doors. 
Each door is controlled by a computer and requires a password to open. 
Unfortunately, our undercover agents have not been able to obtain the secret passwords for the vault doors, but one of our junior agents obtained the source code for each vault's computer! 
You will need to read the source code for each level to figure out what the password is for that vault door. As a warmup, we have created a replica vault in our training facility.

## 🧠 Step 1 - Inspect the Java script

Looking at VaultDoorTraining.java:

```java
import java.util.*;

class VaultDoorTraining {
    public static void main(String args[]) {
        VaultDoorTraining vaultDoor = new VaultDoorTraining();
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

    // The password is below. Is it safe to put the password in the source code?
    // What if somebody stole our source code? Then they would know what our
    // password is. Hmm... I will think of some ways to improve the security
    // on the other doors.
    //
    // -Minion #9567
    public boolean checkPassword(String password) {
        return password.equals("w4rm1ng_Up_w1tH_jAv4_000wYdiGTvt");
    }
}

```
We can see that it appears to be a simple Java program that checks if the password input is correct.

The password is visibly seen in plaintext.

## 🧩 Step 2 - Input the Password

<img width="392" height="35" alt="7743cc5fc863bc1ecc39229ed7487924" src="https://github.com/user-attachments/assets/36c4c0a4-fb98-45f2-80e1-02427e96d7ab" />

Ran the program and with the prefix (picoCTF{}) I was granted access!

## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 25th, 2025  

