# hashcrack - Writeup

## Description

![Alt text](img/1.png)

## solving process

![Alt text](gif/hashcrack.gif)

## Solution

In this challenge we are given three hashes in which we should crack them to get the flag .
For this mission we gonna use hashid ,, hashcat along with rockyou.txt

### Cracking the first hash

![Alt text](img/2.png)

### Cracking the second hash

![Alt text](img/3.png)

### Cracking the third hash

![Alt text](img/4.png)

### Complete Solution / flag

![Alt text](img/5.png)

## solver

```python
#!/usr/bin/env python3
from pwn import *
from hashid import HashID
import hashlib

HOST = 'verbal-sleep.picoctf.net'
PORT = 51759

io = remote(HOST, PORT)
rockyou_path = "/usr/share/wordlists/rockyou.txt"





# first hash

io.recvuntil(b'We have identified a hash: ')
hash = io.recvline().decode().strip()
log.success(f"hash : {hash}")

hashid = HashID()
results = hashid.identifyHash(hash)

for r in results:
    print(f"Possible algorithm: {r.name}")


def crack_md5(md5_hash, wordlist_path):
    with open(wordlist_path, 'r', encoding='latin-1') as file:
        for line in file:
            word = line.strip()
            if hashlib.md5(word.encode('utf-8')).hexdigest() == md5_hash.lower():
                return word
    return None





password = crack_md5(hash, rockyou_path)
if password:
    print(f"Password cracked: {password}")
else:
    print("Password not found in wordlist.")
io.sendline(password.encode())




# second hash


io.recvuntil(b'Flag is yet to be revealed!! Crack this hash: ')
hash = io.recvline().decode().strip()
log.success(f"hash : {hash}")

hashid = HashID()
results = hashid.identifyHash(hash)

for r in results:
    print(f"Possible algorithm: {r.name}")


def crack_sha1(sha1_hash, wordlist_path):
    with open(wordlist_path, 'r', encoding='latin-1') as file:
        for line in file:
            word = line.strip()
            if hashlib.sha1(word.encode('utf-8')).hexdigest() == sha1_hash.lower():
                return word
    return None

password = crack_sha1(hash, rockyou_path)
if password:
    print(f"Password cracked: {password}")
else:
    print("Password not found in wordlist.")
io.sendline(password.encode())


# third hash



io.recvuntil(b'Almost there!! Crack this hash: ')
hash = io.recvline().decode().strip()
log.success(f"hash : {hash}")

hashid = HashID()
results = hashid.identifyHash(hash)

for r in results:
    print(f"Possible algorithm: {r.name}")

def crack_sha256(sha256_hash, wordlist_path):
    with open(wordlist_path, 'r', encoding='latin-1') as file:
        for line in file:
            word = line.strip()
            if hashlib.sha256(word.encode('utf-8')).hexdigest() == sha256_hash.lower():
                return word
    return None

password = crack_sha256(hash, rockyou_path)
if password:
    print(f"Password cracked: {password}")
else:
    print("Password not found in wordlist.")
io.sendline(password.encode())



io.recvuntil(b'The flag is: ')

flag = io.recvline().decode().strip()
log.success(f"FLAG : {flag}")


```

## flag

```
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_ce730f64}
```
