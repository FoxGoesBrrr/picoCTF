### [two-sum](https://play.picoctf.org/practice/challenge/382)

![Name?](https://c.tenor.com/kHcmsxlKHEAAAAAC/rock-one-eyebrow-raised-rock-staring.gif)

When we launch the instance, we will get a source file for a C program. It is a program which compared two numbers. Here we need to get the integer to overflow in order to get the flag.

To do that, lets check the max value that `int` datatype can take. It is `2147483647`. So lets give `2147483648` as one input and some random number as other one.

```sh
n1 > n1 + n2 OR n2 > n1 + n2
What two positive numbers can make this possible:
2147483648
1
You entered -2147483648 and 1
No overflow
```

We don't get any overflow. Lets give the number itself and try

```sh
n1 > n1 + n2 OR n2 > n1 + n2
What two positive numbers can make this possible:
2147483647
5
You entered 2147483647 and 5
You have an integer overflow
YOUR FLAG IS: picoCTF{Tw0_Sum_Integer_Bu773R_0v3rfl0w_76f333c8}
```

Here we got the flag!

Flag: `picoCTF{Tw0_Sum_Integer_Bu773R_0v3rfl0w_76f333c8}`
