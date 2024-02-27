The challenge prompts us to connect to a server using netcat.

    jupiter.challenges.picoctf.org 43522

When we connect to the server, we will recieve a ciphertext.
    
    guvcfaej nqfq lj dubf ozac - ofqkbqvgd_lj_g_uwqf_zasmya_ucosabvfao

    nawlvc nay jusq elsq ae sd yljtujaz pnqv lv zuvyuv, l nay wljleqy enq mfleljn sbjqbs, avy sayq jqafgn asuvc enq muuhj avy satj lv enq zlmfafd fqcafylvc efavjdzwavla; le nay jefbgh sq enae jusq oufqhvupzqycq uo enq gubvefd gubzy nafyzd oalz eu nawq jusq lstufeavgq lv yqazlvc plen a vumzqsav uo enae gubvefd. l olvy enae enq yljeflge nq vasqy lj lv enq qxefqsq qaje uo enq gubvefd, rbje uv enq mufyqfj uo enfqq jeaeqj, efavjdzwavla, suzyawla avy mbhuwlva, lv enq slyje uo enq gaftaenlav subvealvj; uvq uo enq plzyqje avy zqaje hvupv tufeluvj uo qbfutq. l paj vue amzq eu zlcne uv avd sat uf pufh clwlvc enq qxage zugazled uo enq gajezq yfagbza, aj enqfq afq vu satj uo enlj gubvefd aj dqe eu gustafq plen ubf upv ufyvavgq jbfwqd satj; mbe l oubvy enae mljeflei, enq tuje eupv vasqy md gubve yfagbza, lj a oalfzd pqzz-hvupv tzagq. l jnazz qveqf nqfq jusq uo sd vueqj, aj enqd sad fqofqjn sd sqsufd pnqv l eazh uwqf sd efawqzj plen slva.

We can put this in a cipher detector available online. This is most likely a substitution cipher. After using an [online tool](https://www.guballa.de/substitution-solver) we will get the flag. But the flag is not to be submitted in the usual formal, so we are going to submit it as is.

Flag: `frequency_is_c_over_lambda_ogfmaunraf`
