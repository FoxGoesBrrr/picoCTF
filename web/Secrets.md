### [Secrets](https://play.picoctf.org/practice/challenge/296)

In this challenge, we look at the page's source code to find endpoints.

First we are led to a [homepage](http://saturn.picoctf.net:65455/) of a website. By inspecting the page's source, we can see that the stylesheet reference link is `/secret/assets/index.css`. 

Now if we go to `http://saturn.picoctf.net:65455/secret/assets/index.css`, we won't find anything. So let's go to `http://saturn.picoctf.net:65455/secret` instead. By vieweing this page's source, we can see that the stylesheet reference link is `/hidden/file.css`. 

Again we are not supposed to check the `file.css`, but we go to `http://saturn.picoctf.net:65455/secret/hidden` instead. Here we will find a new endpoint `/superhidden`. 

In `http://saturn.picoctf.net:65455/secret/hidden/superhidden` we will the find the flag.

Flag: `picoCTF{succ3ss_@h3n1c@10n_39849bcf}`
