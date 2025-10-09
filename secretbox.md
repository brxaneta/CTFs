# ?? CTF Challenge — Secret Box
?? Challenge Files

- secret.png

- secretbox.py

## ?????? Description

We are provided with a Python script and an image file. The task is to extract the hidden flag from the image.

## ?? Step 1 — Inspect the Python script

Looking at secretbox.py:

```py
import sys
from PIL import Image

def prob(s_img, msg, d_img):
    im = Image.open(s_img).convert("RGBA")
    p = im.load()
    c = 0
    msg = map(lambda x: ord(x) ^ len(d_img), msg[::-1])
    for i in range(0, len(msg)):
        enc = msg[i]
        p[c, 0] = (p[c, 0][0], p[c, 0][1], p[c, 0][2], enc)
        c += 1
    im.save(d_img)

if len(sys.argv) != 4:
    print("%s \"original.png\" \"secret message\" \"secret.png\"" % sys.argv[0])
    exit()

prob(sys.argv[1], sys.argv[2], sys.argv[3])
```
We can tell it encodes a message into the alpha channel (p[c, 0][3]) of an image, using XOR encryption.

The key used for the XOR is the length of the output filename (len(d_img)).

## ?? Step 2 — Understanding the Encoding

The line:
```py
msg = map(lambda x: ord(x) ^ len(d_img), msg[::-1])
```
reveals two things:

- The message is reversed before encoding.

- Each character’s ASCII value is XOR’ed with the length of the output file name.

Since the provided image is called secret.png,
len("secret.png") = 10,
so the XOR key = 10.

## ?? Step 3 — Writing the Decoder

To reverse the process, we can extract the alpha channel, XOR each byte with 10, and reverse the result:
```py
from PIL import Image

def decode(img_path):
    im = Image.open(img_path).convert("RGBA")
    p = im.load()
    msg = []
    key = len("secret.png")  # XOR key
    for c in range(im.width):
        a = p[c, 0][3]
        if a == 0:
            break
        msg.append(chr(a ^ key))
    msg = msg[::-1]  # reverse back
    return ''.join(msg)

print(decode("secret.png"))
```