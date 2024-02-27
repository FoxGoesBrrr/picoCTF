### [hideme](https://play.picoctf.org/practice/challenge/350)

We are introduced to an image named `flag.png`. Lets use the command `strings` on this file.

    strings flag.png

In the ouput, we can see that there is a folder named `secret` and a file named `flag.png` (yes another one) inside the folder. Lets use `binwalk` to extract the image.

    binwalk -e flag.png

Now in the secrets folder inside our extracted folder, we can find the `flag.png` file which has the flag.

Flag: `picoCTF{Hiddinng_An_imag3_within_@n_ima9e_dc2ab58f}`
