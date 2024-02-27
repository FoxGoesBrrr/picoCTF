### [unpackme](https://play.picoctf.org/practice/challenge/313)

This is a challenge where we use tools like binary ninja or ghidra to disassemble an application. Here we recieve a file named `unpackme-upx`. Before we run the file, we need to give it the permissions to be executable.

```sh
chmod +x unpackme-upx
```

We will get a prompt asking us `What's my favorite number?`. Not like we can guess its favorite number, lets disassemble it. By verifying its hex data with [file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures), we will know that its a `.upx` compressed file (it is also given in the name of the file itself). Using the `upx` tool, we can decompress it.

```sh
upx -kd unpackme-upx
```

And then by using ghidra, we can disassemble the decompressed application. Lets check the `main` function.

```assembly
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
```

Here, the code is checking if the number we gave is `0xb83cb` or not. This is hexadecimal for the number `754635`. When we give this number to the application, we will get the flag.

Flag: `picoCTF{up><_m3_f7w_77ad107e}`
