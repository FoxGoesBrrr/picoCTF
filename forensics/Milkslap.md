### [Milkslap](https://play.picoctf.org/practice/challenge/139)

When we open the challenge page, we will notice a milk glass emoji, which is a redirection to a website. In the website, we will find a moving png, also known as `apng` whose extension is same as a normal `png` file. Lets download this and check its data.

In the properties tab, we don't find anything. Lets run `strings` command on this.

```sh
strings concat_v.png
strings concat_v.png | grep pic
strings concat_v.png | grep picoCTF
```

We can't find any data matching our search string. Lets use `zsteg` to analyse the image.

```sh
zsteg concat_v.png
```

Ran into some memories issues with ruby but finally we will be able to get the flag from this command itself.

Flag: `picoCTF{imag3_m4n1pul4t10n_sl4p5}`
