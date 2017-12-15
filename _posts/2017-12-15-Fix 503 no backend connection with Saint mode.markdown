---
layout: post
title:  "Fix 503 no backend connection with Saint mode"
date:   2017-12-15 12:34:07 +0000
categories: varnish saintmode saintmode_threshold
---
If you are getting allot 503 FetchError c no backend connection error and you enabled Saint mode than you have to add following to your backend.


.saintmode_threshold = 0;

Option "saintmode_threshold" tells Varnish how ,any items can be vlacklisted by saint mode before it makes your backend sick. Is setting saintmode threshold to 0 still produces "no backend errors" try sett try setting it to a very high number.

To check if you have such error use following command


# varnishlog -c -m TxStatus:503 | grep FetchError
