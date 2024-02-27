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
