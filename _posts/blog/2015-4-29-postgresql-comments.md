---
layout: post
title: Viewing column-comments in PostgreSQL
tags: [nerd-out]
---

I'm not sure why, but any time someone asks for help viewing column-comments in PostgreSQL, the entire internet responds with a super-complex custom function. 

This is how you view all the comments for each column of a table:

    \d+ <table_name>

The end.