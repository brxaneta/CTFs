# 🧩 CTF Challenge - flag_hunters
📁 Challenge Resources

- lyric-reader.py


## 🕵️‍♂️ Description

Lyrics jump from verses to the refrain kind of like a subroutine call. There's a hidden refrain this program doesn't print by default. Can you get it to print it? 
There might be something in it for you.

<img width="330" height="163" alt="7ec97c956d706e420072611fbb96cb1e-1" src="https://github.com/user-attachments/assets/3987ae37-a199-4eaa-84bd-5c44b67dabc0" />


## Inspect the Source Code

The secret_intro verse contains our flag:
```python
secret_intro = \
'''Pico warriors rising, puzzles laid bare,
Solving each challenge with precision and flair.
With unity and skill, flags we deliver,
The ether's ours to conquer, '''\
+ flag + '\n'
```

The code prepends secret_intro to the top of song_flag_hunter:
```python
song_flag_hunters = secret_intro + \
```

…but the reader is invoked starting from [VERSE1]:
```python
reader(song_flag_hunters, '[VERSE1]')
```

So our goal is to make the reader begin at the very first line (where the flag verse lives).

## How the Injection Works
The script takes unsanitized user input in the CROWD branch:
```python
elif re.match(r"CROWD.*", line):
    crowd = input('Crowd: ')
    song_lines[lip] = 'Crowd: ' + crowd
    lip += 1
```
There’s also a command the reader recognizes that jumps the read pointer:
```python
elif re.match(r"RETURN [0-9]+", line):
    lip = int(line.split()[1])
```
If we send RETURN 0 as crowd input, the program will treat it as data and just echo it back. 
To force the script to interpret it as a command, we inject it as part of the input so the parser will see a command token rather than only a literal line.

## Payload
Use a payload that appends the command, for example:
```python
some_string;RETURN 0
```
This places a RETURN 0 token where the reader will parse it as a command, setting lip = 0 and forcing the reader to start at the very first line - revealing the flag verse added by secret_intro.

<img width="324" height="263" alt="198a9352b67444fddf20a9c2c2ed7601" src="https://github.com/user-attachments/assets/d22f8c3e-258e-4911-9bca-0a7f43194c73" />

## 🤯 Pwned

**Author:** Brian Araneta 

**Date:** November 25th, 2025  
