Given ciphertext:
```
heTfl g as iicpCTo{7F4NRP051N5_16_35P3X51N3_VCDE4CE4}7
```
This is a transposition cipher where the positions of the characters in the plaintext are changed. 
This can be solved with a simple Python script:

```py
ct = "heTfl g as iicpCTo{7F4NRP051N5_16_35P3X51N3_VCDE4CE4}7"
pt = ""
x = len(ct)//3

for i in range(x):
    a = ct[3*i]
    a += ct[3*i+1]
    a = ct[3*i+2]+a
    pt += a

print(pt)
```

The flag is: `picoCTF{7R4N5P051N6_15_3XP3N51V3_ECDE4C74}`
