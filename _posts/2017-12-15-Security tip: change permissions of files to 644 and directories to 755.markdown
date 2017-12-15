---
layout: post
title:  "Security tip: change permissions of files to 644 and directories to 755"
date:   2017-12-15 12:34:07 +0000
categories: jekyll update
---

For files use:


user@server:~/web$ find ~ -type f -exec chmod 644 {} \;

For directories use:


user@server:~/web$ find ~ -type d -exec chmod 755 {} \;
