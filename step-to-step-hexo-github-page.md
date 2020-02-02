---
title: step to step hexo github page
date: 2018-04-26 22:31:15
tags:
---
All under the linuxOS
Require:git nodejs
1.register github account https://github.com
2.click person home page right up plus icon and new repository
3.if your account name is bbb, the repository name better is bbb.githut.io ,others is ok but web root will be bbb.github.io/other
4.npm install hexo-cli -g
5.hexo init & npm install
option:
6.cmd: ssh-keygen -t rsa -C "mailname@mail.com"
7.get into the project settings select the deploy keys 
8.add the key from the step 6 generate pub key, the name do with your pleasure, and check the access option
9.bash test: bash ssh -T git@github.com
a.modify the _config.yml make sure the deploy repo is ssh format
b.(hexo g &hexo d) || hexo d -g

hexo cmd:
hexo g: generate static web resource for web display
hexo s: start local hexo server
hexo d: direct deploy to the remote machine
