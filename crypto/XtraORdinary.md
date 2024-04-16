We are given two files. The contents of the files are:
	
output.txt:
	
~~~
57657535570c1e1c612b3468106a18492140662d2f5967442a2960684d28017931617b1f3637
~~~

encrypt.py

~~~py
#!/usr/bin/env python3

from random import randint

with open('flag.txt', 'rb') as f:
    flag = f.read()

with open('secret-key.txt', 'rb') as f:
    key = f.read()

def encrypt(ptxt, key):
    ctxt = b''
    for i in range(len(ptxt)):
        a = ptxt[i]
        b = key[i % len(key)]
        ctxt += bytes([a ^ b])
    return ctxt

ctxt = encrypt(flag, key)

random_strs = [
    b'my encryption method',
    b'is absolutely impenetrable',
    b'and you will never',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'ever',
    b'break it'
]

for random_str in random_strs:
    for i in range(randint(0, pow(2, 8))):
        for j in range(randint(0, pow(2, 6))):
            for k in range(randint(0, pow(2, 4))):
                for l in range(randint(0, pow(2, 2))):
                    for m in range(randint(0, pow(2, 0))):
                        ctxt = encrypt(ctxt, random_str)

with open('output.txt', 'w') as f:
    f.write(ctxt.hex())
~~~

This is just a set of XORs. The flag can be deciphered if we can go through all the possible iterations. This can be done using the following python code.

~~~py
import binascii

ctflag = binascii.unhexlify("57657535570c1e1c612b3468106a18492140662d2f5967442a2960684d28017931617b1f3637")

def encrypt(ptxt, key):
    ctxt = b''
    for i in range(len(ptxt)):
        a = ptxt[i]
        b = key[i % len(key)]
        ctxt += bytes([a ^ b])
    return ctxt

random_strs = [
    b'my encryption method',
    b'is absolutely impenetrable',
    b'and you will never',
    b'ever',
    b'break it'
]

if __name__ == "__main__":
    solutions = [ctflag]
    for rstring in random_strs:
        for i in range(len(solutions)):
            ctxt = encrypt(solutions[i],rstring)
            solutions.append(ctxt)
    solutions = list(set(solutions))
    keys = []
    for i in range(len(solutions)):
        newkey = encrypt(solutions[i],b"picoCTF{")
        newkey = newkey[:7]
        keys.append(newkey)
    keys = list(set(keys))
    flags = []
    for key in keys:
        for soln in solutions:
            newflag = encrypt(soln,key).decode()
            if ("picoCTF{" in newflag) and ("}" in newflag):
                print(newflag)
                flags.append(newflag)
~~~
When we run the code, it will print the flag as is.

Flag: 
`picoCTF{w41t_s0_1_d1dnt_1nv3nt_x0r???}`
