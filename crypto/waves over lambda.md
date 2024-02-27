The challenge prompts us to connect to a server using netcat.

```sh
nc jupiter.challenges.picoctf.org -p 43522
```

When we connect to the server, we will recieve a ciphertext.
    
    guvcfaej nqfq lj dubf ozac - ofqkbqvgd_lj_g_uwqf_zasmya_ucosabvfao

    nawlvc nay jusq elsq ae sd yljtujaz pnqv lv zuvyuv, l nay wljleqy enq mfleljn sbjqbs, avy sayq jqafgn asuvc enq muuhj avy satj lv enq zlmfafd fqcafylvc efavjdzwavla; le nay jefbgh sq enae jusq oufqhvupzqycq uo enq gubvefd gubzy nafyzd oalz eu nawq jusq lstufeavgq lv yqazlvc plen a vumzqsav uo enae gubvefd. l olvy enae enq yljeflge nq vasqy lj lv enq qxefqsq qaje uo enq gubvefd, rbje uv enq mufyqfj uo enfqq jeaeqj, efavjdzwavla, suzyawla avy mbhuwlva, lv enq slyje uo enq gaftaenlav subvealvj; uvq uo enq plzyqje avy zqaje hvupv tufeluvj uo qbfutq. l paj vue amzq eu zlcne uv avd sat uf pufh clwlvc enq qxage zugazled uo enq gajezq yfagbza, aj enqfq afq vu satj uo enlj gubvefd aj dqe eu gustafq plen ubf upv ufyvavgq jbfwqd satj; mbe l oubvy enae mljeflei, enq tuje eupv vasqy md gubve yfagbza, lj a oalfzd pqzz-hvupv tzagq. l jnazz qveqf nqfq jusq uo sd vueqj, aj enqd sad fqofqjn sd sqsufd pnqv l eazh uwqf sd efawqzj plen slva.

We can put this in a cipher detector available online. This is most likely a substitution cipher. After using an [online tool](https://www.guballa.de/substitution-solver) we will get the flag. But the flag is not to be submitted in the usual formal, so we are going to submit it as is. The output we get is:

    congrats here is your flag - frequency_is_c_over_lambda_ogfmaunraf
    
    having had some time at my disposal when in london, i had visited the british museum, and made search among the books and maps in the library regarding transylvania; it had struck me that some foreknowledge of the country could hardly fail to have some importance in dealing with a nobleman of that country. i find that the district he named is in the extreme east of the country, just on the borders of three states, transylvania, moldavia and bukovina, in the midst of the carpathian mountains; one of the wildest and least known portions of europe. i was not able to light on any map or work giving the exact locality of the castle dracula, as there are no maps of this country as yet to compare with our own ordnance survey maps; but i found that bistritz, the post town named by count dracula, is a fairly well-known place. i shall enter here some of my notes, as they may refresh my memory when i talk over my travels with mina.

Flag: `frequency_is_c_over_lambda_ogfmaunraf`
