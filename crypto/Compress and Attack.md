We are given a python file `compress_and_attack.py`, it has the following content.
	
~~~py
#!/usr/bin/python3 -u

import zlib
from random import randint
import os
from Crypto.Cipher import Salsa20

flag = open("./flag").read()


def compress(text):
    return zlib.compress(bytes(text.encode("utf-8")))

def encrypt(plaintext):
    secret = os.urandom(32)
    cipher = Salsa20.new(key=secret)
    return cipher.nonce + cipher.encrypt(plaintext)

def main():
    while True:
        usr_input = input("Enter your text to be encrypted: ")
        compressed_text = compress(flag + usr_input)
        encrypted = encrypt(compressed_text)
        
        nonce = encrypted[:8]
        encrypted_text =  encrypted[8:]
        print(nonce)
        print(encrypted_text)
        print(len(encrypted_text))

if __name__ == '__main__':
    main()
~~~

This code snippet processes a user text string by appending it to a flag. It then compresses the combined string using zlib.compress and encrypts it with Salsa20, utilizing a randomly generated 32-byte unsigned integer key. Subsequently, the script assigns the first 8 characters to the variable `nonce` and the remaining characters to the variable `encrypted_text`, both of which are then printed.

In Python, we can obtain the nonce, encrypted text, and the length of the encrypted text. It's important to note that, as the response is a string of bytes, we must handle corrections for both hexadecimal and ASCII characters.

~~~py
import socket
import time
import binascii

URL = "mercury.picoctf.net"
PORT = 29858
PT_TEST = "aaaaaaaaaaaaaaaaaaaa"

def str2bytes(messy_string):
    output_string = ""
    messy_array = messy_string.split("\\x")
    for i in range(len(messy_array)):
        if len(messy_array[i])>=2:
            hexletters = messy_array[i][0:2]
            output_string += hexletters
            if len(messy_array[i])>=3:
                letters = messy_array[i][2:]
                hexletters = binascii.hexlify(letters.encode()).decode()
                output_string += hexletters
        else:
            letters = messy_array[i]
            hexletters = binascii.hexlify(letters.encode()).decode()
            output_string += hexletters
    output_bytes = binascii.unhexlify(output_string)
    return output_bytes

def load_flag():
    global CT_MSG
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((URL,PORT))
    time.sleep(1)
    recvbuff = s.recv(1000).decode()
    s.send((PT_TEST+"\n").encode())
    time.sleep(1)
    recvbuff = s.recv(1000).decode()
    print(recvbuff+"\n")
    recvbuff = recvbuff.splitlines()
    noncebuff = recvbuff[0][2:-1]
    nonce = str2bytes(noncebuff)
    enc_text_buff = recvbuff[1][2:-1]
    enc_text = str2bytes(enc_text_buff)
    enc_text_len = int(recvbuff[2])
    s.close()
    return nonce, enc_text, enc_text_len

if __name__ == "__main__":
    a, b, c = load_flag()
    print("nonce = {}\n".format(a))
    print("enc_text = {}\n".format(b))
    print("enc_text_length = {}".format(c))
~~~

This provides the following output:

~~~
b'\xe02\x91\x95\x0b\x0c\xd9\x91'
b"\xd7!I\xd8s\x05Wx\xc6\x95E\xf0?\x98kp\xe1'\xe1\xefq`\xe1\xb4\xf2(\xb2\xb5Ttmx\xa310\x8fW\xaaS\xda_\xb5|0\x07~\x82"
47
Enter your text to be encrypted: 

nonce = b'\xe02\x91\x95\x0b\x0c\xd9\x91'

enc_text = b"\xd7!I\xd8s\x05Wx\xc6\x95E\xf0?\x98kp\xe1'\xe1\xefq`\xe1\xb4\xf2(\xb2\xb5Ttmx\xa310\x8fW\xaaS\xda_\xb5|0\x07~\x82"

enc_text_length = 47
~~~

The Salsa20 stream cipher allows us to estimate the length of the plaintext input from the ciphertext output. Employing zlib compression allows us to search for repeatable characters that do not significantly increase the length of the ciphertext response.

We can verify this functionality using netcat for testing purposes.

~~~shell
$ nc mercury.picoctf.net 29858
Enter your text to be encrypted: picoCTFa
b'\xf2\xfe\x8e\x18\xde\xa8K\xbc'
b"o\n\xbb\x0c\x81\xed\xd4\xe9\xfa\xaf'e\xccs\xbd\xed\xf2\xc1BmR\x1f\xde\xb2R0W\xcc$\x99\x83\x8e\x90k\xe9\x13\x19\xbd\xf8I\xc1!\xbcgf\xa7\xa8\x92\x81"
49

Enter your text to be encrypted: picoCTF{
b'\xf4\xdc\xcf~\x9a\xa3\x97\x13'
b'\xa6\xed\xd4C\xbfS7a\'\xe9\xd5\xd2\xb6\x16\x12\x8d\x1f\xef\xe2\xe1\x85=\xca\xfa\xd20\x97\x1e\xa3\xdb\xc7>\xc4&\xdaE\xbe3K\x89\xc7\x96ec\xcff"K'
48
~~~

Using Python, we can develop a script that iterates through the permitted characters and adds the correct characters to a flag string. This process ensures that the encrypted response does not increase in length.

~~~py
import socket
import time

URL = "mercury.picoctf.net"
PORT = 29858
FLAG = "picoCTF{"
alphabet = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ_{}"

def reconnect(s):
    if(s):
        s.close()
    time.sleep(5)
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((URL, PORT))
    time.sleep(1)
    s.recv(1000).decode()
    return s

def find_flag():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((URL, PORT))
    time.sleep(1)
    recvbuff = s.recv(1000).decode()
    global FLAG
    while(FLAG[:-1]!="}"):
        try:
            s.send((FLAG+"\n").encode())
            time.sleep(0.5)
            recvbuff = s.recv(1000).decode()
            recvbuff = recvbuff.splitlines()
            reflen = int(recvbuff[2])
        except:
            s = reconnect(s)
        for letter in alphabet:
            test = FLAG + letter
            try:
                s.send((test+"\n").encode())
                time.sleep(0.5)
                recvbuff = s.recv(1000).decode()
                recvbuff = recvbuff.splitlines()
                testlen = int(recvbuff[2])
                if testlen == reflen:
                    FLAG += letter
                    print(FLAG)
                    break
            except:
                s = reconnect(s)
    return FLAG

if __name__ == "__main__":
    flag = find_flag()
    print(flag)
~~~


Flag: 
`picoCTF{sheriff_you_solved_the_crime}`



