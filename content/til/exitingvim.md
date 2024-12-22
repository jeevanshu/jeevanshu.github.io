---
title: Save and quit vim
date: 2024-08-06T11:45:26+05:30
---

It has been a long running computer science joke that once you open vim its impossible to quit it. Its also one of the most popularly searched questions on stackoverflow https://stackoverflow.com/questions/11828270/how-do-i-exit-vim.

I interact alot with vim during my work and have always relied on command `:wq` as a means of saving and exiting the editor. But I as a course of learning more efficient key bindings in vim I came across two new commands that I hadnt used before `:x` and `ZZ`

While `:wq` always saves and exits vim, even  if there were no changes. So, this command always updates the timestamp of the file.

`:x` is a little more smarter. It saves only if there were changes made, if there are no changes then just exits the file.

`ZZ` is same as `:x` command but its a normal mode command i.e. you dont need to enter command mode by pressing `:`, saving you few precious key strokes.

I learnt a lot of vim bindings by using `vimtutor`, I highly recommend anyone who wants to enter linux administration adjacent roles to give it a try and become familiar with all powerful shortcuts that vim offers