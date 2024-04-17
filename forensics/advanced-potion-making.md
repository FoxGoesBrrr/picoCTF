A [file](https://artifacts.picoctf.net/picoMini+by+redpwn/Forensics/advanced-potion-making/advanced-potion-making) was provided with the question. We can use any hex editor to see with what is starts..
~~~
89 50 42 11 0d 0a 1a 0a 00 12 13 14 49 48 44 52
.  P  B  .  .  .  .  .  .  .  .  .  I  H  D  R
~~~

`89 50`? Isn't it the file signature of PNG file? Let's rename it.

~~~shell
cp advanced-potion-making avp_new.png
~~~

But the header still looks broken, it should be `89 50 4E 47 0D 0A 1A 0A` not `89 50 42 11 0D 0A 1A 0A`. So let's change that too.

~~~
89 50 4e 47 0d 0a 1a 0a 
.  P  M  G  .  .  .  .  
~~~

Let's use `strings` to see if we can find anything useful.

~~~shell
strings -n 7 -t x avp_new.png 
     55 v9IDATx^
   149b Wd`	<D~F
   16e6 H2o)7}(
   185d {}]jY.7h(
   1a05 J9o)7}(
   1b10 )^ENq@ix(
   2420 |RBNr;N
   2dff 'Yv.[<N
   33fb 4icRB>e
   391d R{@ix^r
   3e7b OY3#	7I
   4062 l=#	'Y6
   425f 40#	'Yv
   4803 K{<T.~d
   4a34 zwJ8<S>
   5606 S~:_s.Q
   5663 Oyb>Gz8
~~~

Let's use `binwalk` to see if we can find anything useful like we did in the previous challenge.

~~~shell
binwalk -Me avp_new.png 

Scan Time:     2024-04-08 09:22:18
Target File:   /home/foxy/Downloads/avp_new.png
MD5 Checksum:  54be76cbb51bde77f453f8c64ed407a4
Signatures:    391

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 2448 x 1240, 8-bit/color RGB, non-interlaced
91            0x5B            Zlib compressed data, compressed


Scan Time:     2021-04-08 09:22:18
Target File:   /home/foxy/Downloads/_avp_new.png.extracted/5B
MD5 Checksum:  e8789f9ed022607cd1ef037bc8511621
Signatures:    391

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
~~~

**I hate Forensics.** Let's use some online tools which will do the steganography for me. [StegOnline](https://stegonline.georgeom.net) has nice tools for this job, if we look at the bit planes part, we can see the flag.

Flag: `picoCTF{w1z4rdry}`
