---
title: git建立ssh连接
date: 2017-05-15 21:37:03
tags:
---
1. 命令行输入 ssh-keygen -t rsa -C "github注册时的邮箱"  
2. ssh-agent -s
3. eval `ssh-agent -s`
   ssh-add 
4. ssh-add ~/.ssh/id_rsa
5. clip < ~/.ssh/id_rsa.pub
6. 然后到Github里面，点击右上角的设置图标Settings,找到SSH keys,Key就直接粘贴,  
   然后点Add SSH key，最后会让你重新输入下gitHub的密码  
7. ssh -T git@github.com  
8. 输入yes  
所有指令均在git-bash中输入  
