# 🧩 CTF Challenge - Vault Door 8
Category: Reverse Engineering

📁 Challenge Resources

- VaultDoor8.java

## 🕵️‍♂️ Description
Apparently Dr. Evil's minions knew that our agency was making copies of their source code, because they intentionally sabotaged this source code in order to make it harder for our agents to analyze and crack into! 
The result is a quite mess, but I trust that my best special agent will find a way to solve it.

The challenge provides a Java program (VaultDoor8.java) that checks part of the user’s input by running each character through a scramble() bit-swapping function. 
The scrambled bytes are compared against a hard-coded expected[] array. 
Our goal is to reverse the scrambling process to recover the original substring that forms the core of the flag.

## The Code
```java
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891
import java.util.*; import javax.crypto.Cipher; import javax.crypto.spec.SecretKeySpec;
import java.security.*; class VaultDoor8 {public static void main(String args[]) {
Scanner b = new Scanner(System.in); System.out.print("Enter vault password: ");
String c = b.next(); String f = c.substring(8,c.length()-1); VaultDoor8 a = new VaultDoor8(); if (a.checkPassword(f)) {System.out.println("Access granted."); }
else {System.out.println("Access denied!"); } } public char[] scramble(String password) {/* Scramble a password by transposing pairs of bits. */
char[] a = password.toCharArray(); for (int b=0; b<a.length; b++) {char c = a[b]; c = switchBits(c,1,2); c = switchBits(c,0,3); /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */ c = switchBits(c,5,6); c = switchBits(c,4,7);
c = switchBits(c,0,1); /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */ c = switchBits(c,3,4); c = switchBits(c,2,5); c = switchBits(c,6,7); a[b] = c; } return a;
} public char switchBits(char c, int p1, int p2) {/* Move the bit in position p1 to position p2, and move the bit
that was in position p2 to position p1. Precondition: p1 < p2 */ char mask1 = (char)(1 << p1);
char mask2 = (char)(1 << p2); /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */ char bit1 = (char)(c & mask1); char bit2 = (char)(c & mask2); /* System.out.println("bit1 " + Integer.toBinaryString(bit1));
System.out.println("bit2 " + Integer.toBinaryString(bit2)); */ char rest = (char)(c & ~(mask1 | mask2)); char shift = (char)(p2 - p1); char result = (char)((bit1<<shift) | (bit2>>shift) | rest); return result;
} public boolean checkPassword(String password) {char[] scrambled = scramble(password); char[] expected = {
0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xF1, 0xC2, 0xD1, 0xB4, 0xD1, 0xB4, 0xF1, 0xF1, 0x85 }; return Arrays.equals(scrambled, expected); } }
```
As you can see, the code is insanely jumbled and hard to read.
So, I used Code Beautifier which cleaned it up nicely:
```java
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891
import java.util.*;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.security.*;
class VaultDoor8 {
  public static void main(String args[]) {
    Scanner b = new Scanner(System.in);
    System.out.print("Enter vault password: ");
    String c = b.next();
    String f = c.substring(8, c.length() - 1);
    VaultDoor8 a = new VaultDoor8();
    if (a.checkPassword(f)) {
      System.out.println("Access granted.");
    } else {
      System.out.println("Access denied!");
    }
  }
  public char[] scramble(String password) {
    /* Scramble a password by transposing pairs of bits. */
    char[] a = password.toCharArray();
    for (int b = 0; b < a.length; b++) {
      char c = a[b];
      c = switchBits(c, 1, 2);
      c = switchBits(c, 0, 3); /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */
      c = switchBits(c, 5, 6);
      c = switchBits(c, 4, 7);
      c = switchBits(c, 0, 1); /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */
      c = switchBits(c, 3, 4);
      c = switchBits(c, 2, 5);
      c = switchBits(c, 6, 7);
      a[b] = c;
    }
    return a;
  }
  public char switchBits(char c, int p1, int p2) {
    /* Move the bit in position p1 to position p2, and move the bit
    that was in position p2 to position p1. Precondition: p1 < p2 */
    char mask1 = (char)(1 << p1);
    char mask2 = (char)(1 << p2); /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */
    char bit1 = (char)(c & mask1);
    char bit2 = (char)(c & mask2);
    /* System.out.println("bit1 " + Integer.toBinaryString(bit1));
System.out.println("bit2 " + Integer.toBinaryString(bit2)); */
    char rest = (char)(c & ~(mask1 | mask2));
    char shift = (char)(p2 - p1);
    char result = (char)((bit1 << shift) | (bit2 >> shift) | rest);
    return result;
  }
  public boolean checkPassword(String password) {
    char[] scrambled = scramble(password);
    char[] expected = {
      0xF4,
      0xC0,
      0x97,
      0xF0,
      0x77,
      0x97,
      0xC0,
      0xE4,
      0xF0,
      0x77,
      0xA4,
      0xD0,
      0xC5,
      0x77,
      0xF4,
      0x86,
      0xD0,
      0xA5,
      0x45,
      0x96,
      0x27,
      0xB5,
      0x77,
      0xF1,
      0xC2,
      0xD1,
      0xB4,
      0xD1,
      0xB4,
      0xF1,
      0xF1,
      0x85
    };
    return Arrays.equals(scrambled, expected);
  }
}
```
There - nice and readable. Now we can get to examining.

Only the middle portion of the flag is validated:

```String input = ctf.substring(8, ctf.length() - 1);```

So the actual flag is:

```picoCTF{XXXXXXXX<decoded_substring>Z}```

The first 8 chars and last char are ignored, meaning only the substring between them matters.

## Understanding the Scramble Function
Each character is converted to a byte and has several bit positions swapped. The actual code performs 8 swaps in a fixed order:
```java
bits = switchBits(bits, 1, 2);
bits = switchBits(bits, 0, 3);
bits = switchBits(bits, 5, 6);
bits = switchBits(bits, 4, 7);
bits = switchBits(bits, 0, 1);
bits = switchBits(bits, 3, 4);
bits = switchBits(bits, 2, 5);
bits = switchBits(bits, 6, 7);
```
switchBits() simply swaps two bit positions of an 8-bit value.

To reverse the scrambling, we must apply the swaps in reverse order.

## Reversing the Algorithm
The expected[] array contains 32 scrambled byte values. If we run those bytes backwards through all 8 swaps, we reconstruct the original characters.

I wrote a Python script that implements the swap logic and undoes the scrambling:
```python
def switch_bits(val, p1, p2):
    mask1 = 1 << p1
    mask2 = 1 << p2
    bit1 = val & mask1
    bit2 = val & mask2
    rest = val & ~(mask1 | mask2)
    shift = p2 - p1
    return ((bit1 << shift) | (bit2 >> shift) | rest) & 0xFF

swaps = [
    (1,2),(0,3),(5,6),(4,7),(0,1),(3,4),(2,5),(6,7)
]

inverse_swaps = list(reversed(swaps))

expected = [
    0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4,
    0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86,
    0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xF1,
    0xC2, 0xD1, 0xB4, 0xD1, 0xB4, 0xF1, 0xF1, 0x85
]

recovered = []

for byte in expected:
    v = byte
    for (p1,p2) in inverse_swaps:
        v = switch_bits(v, p1, p2)
    recovered.append(v)

password = "".join(chr(c) for c in recovered)

print("Recovered password:", password)
```
## Result
Running the script produces:

```s0m3_m0r3_b1t_sh1fTiNg_785c5c77d```

BOOM! Flag found!

## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 26th, 2025  
