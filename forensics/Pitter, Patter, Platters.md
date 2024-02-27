### [Pitter, Patter, Platters](https://play.picoctf.org/practice/challenge/87)

We are asked to download a file named `suspicious.dd.sda1`, which is a file system image. Lets mount it and check what's in there.

    mkdir sus
    sudo mount suspicious.dd.sda1 ~/sus

Here we will find a text file named `suspicious-file.txt` which has the following content.

    Nothing to see here! But you may want to look here -->

What are we missing here? We cannot access the `lost+found` directory. After checking all the files, I couldn't find anything. Lets try using `strings` command on this to see its data.

    strings suspicious.dd.sda1
    strings suspicious.dd.sda1 | grep picoCTF

We don't get anything meaningful here. For the second command, we don't even get any output. Lets try opening this with 16 bit encoding.

    strings -e b suspicious.dd.sda1

The output is as follows.

    6b7d549b_3<_|Lm_111t5_3b{FTCocip
    qQWTYUiIOPb
    FGjJKLs
    ZXvc
    #+3;CScs
    !1Aa
    qQWTYUiIOPb
    FGjJKLs
    ZXvc
    #+3;CScs
    !1Aa
    #+3;CScs
    !1Aa
    7777777777

Here we can see the flag but in reverse, so lets turn it.

    strings -e b suspicious.dd.sda1 | rev

The output is

    picoCTF{b3_5t111_mL|_<3_b945d7b6
    bPOIiUYTWQq
    sLKJjGF
    cvXZ
    scSC;3+#
    aA1!
    bPOIiUYTWQq
    sLKJjGF
    cvXZ
    scSC;3+#
    aA1!
    scSC;3+#
    aA1!
    7777777777

Here we have the flag, just add a `}` at the end.

Flag: `picoCTF{b3_5t111_mL|_<3_b945d7b6}`

